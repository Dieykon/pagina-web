---
title: ⚙️ Customize Spring MongoDB DBRef Resolution
date: 2021-02-12 03:00:00 -0400
categories: [Java, Spring]
tags:
  - java
  - spring
---

I think I will not open a secret saying that many developers love [Spring](https://spring.io/) for the ability to quickly make sometimes complex things relatively simple and elegant. A huge community of developers with their questions and answers helps to quickly find a solution to a particular problem.

Today I would like to share one solution that allowed me to significantly improve performance when reading data from MongoDB.

In my scenario, I'm actively using [DBRefs](https://docs.mongodb.com/manual/reference/database-references/) to refer from one object to another instead of embedding objects within a document. Perhaps you can argue and say that it should not be used taking into account that on the MongoDB web site there is

> Unless you have a compelling reason to use DBRefs, use manual references instead.

But my point is if MongoDB has such type, where I can explicitly see the information about collection and ID why don't use it. Another point is my data layer is build on top of [Spring Data MongoDB](https://spring.io/projects/spring-data-mongodb) which simplifies many things one thing is how Spring Data MongoDB mapping framework works with DBRefs

> The mapping framework does not have to store child objects embedded within the document. You can also store them separately and use a DBRef to refer to that document. When the object is loaded from MongoDB, those references are eagerly resolved so that you get back a mapped object that looks the same as if it had been stored embedded within your top-level document.  
> _ **Source** _: [https://docs.spring.io/spring-data/mongodb/docs/3.1.3/reference/html/#mapping-usage-references](https://docs.spring.io/spring-data/mongodb/docs/3.1.3/reference/html/#mapping-usage-references)

And everything is good until you have relatively small objects without deep nesting. When nesting becomes quite deep or when you need to load more data or when DB grows you might notice performance degradation like happened in my scenario, i.e. each time when the mapping framework fetches the data, even if data is repeated it tries to fetch each time ignoring the fact the same portion already fetched from the DB.

To resolve DBRefs by default Spring uses [DefaultDbRefResolver](https://docs.spring.io/spring-data/data-mongodb/docs/current/api/org/springframework/data/mongodb/core/convert/DefaultDbRefResolver.html). Enabling logging for `org.springframework.data.mongodb.core.convert.DefaultDbRefResolver` might be very useful to see what happens.

In my application, I noticed too much `Fetching DBRef...` and `Bulk fetching DBRefs...` which pushed me to move in that direction and come up with a solution to avoid unnecessarily fetching. I decided to create a custom `DbRefResolver` and use [Spring caching](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-caching). Below is my `CachedDbRefReolver`

```java
@Slf4j
public class CachedDbRefResolver extends DefaultDbRefResolver {

    private final Cache cache;

    public CachedDbRefResolver(MongoDatabaseFactory mongoDbFactory, CacheManager cacheManager) {
        super(mongoDbFactory);
        this.cache = cacheManager.getCache(CacheConstants.DBREF_CACHE_NAME);
    }

    @Override
    public Document fetch(@NotNull DBRef dbRef) {
        Document document = cache.get(dbRef.getId().toString(), Document.class);
        if (document == null) {
            document = super.fetch(dbRef);
            cache.put(dbRef.getId().toString(), document);
        }

        return document;
    }

    @Override
    public @NotNull List<Document> bulkFetch(List<DBRef> dbRefs) {
        List<DBRef> missingDbRefs = new ArrayList<>();
        List<Document> result = new ArrayList<>();
        for (DBRef dbRef : dbRefs) {
            Document document = cache.get(dbRef.getId().toString(), Document.class);
            if (document == null) {
                missingDbRefs.add(dbRef);
            } else {
                result.add(document);
            }
        }

        if (!missingDbRefs.isEmpty()) {
            List<Document> missingDocuments = super.bulkFetch(missingDbRefs);
            for (Document missingDocument : missingDocuments) {
                cache.put(missingDocument.get(MongoUtils.UNDERSCORE_ID).toString(), missingDocument);
                result.add(missingDocument);
            }
        }

        return result;
    }
}
```

As you can see I use `DefaultDbRefResolver` as the base and override two methods:

- `fetch` to fetch individual Documents
- `bulkFetch` for bulk fetching to fetch only items, not existing in the cache

In the code above I use a few constants:

- `CacheConstants.DBREF_CACHE_NAME` is `public static final String DBREF_CACHE_NAME = "DBREF";`
- `MongoUtils.UNDERSCORE_ID` is `public static final String UNDERSCORE_ID = "_id";`

My `CacheManager` is `ConcurrentMapCacheManager` registered as

```java
@Configuration
public class CacheBeans {

    @Bean
    public CacheManager cacheManager() {
        return new ConcurrentMapCacheManager(CacheConstants.DBREF_CACHE_NAME);
    }
}
```

See more details here [Supported Cache Providers](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-caching-provider).

Another important point with cache is a cache eviction.

> _There are only two hard things in Computer Science: cache invalidation and naming things._  
> _-- Phil Karlton_

Since I know when the data could be evicted from the cache. In my scenario, I have a custom repository to get access to the data and any update means that an item needs to be evicted. Considering all of the above I see two options:

- remove the documents by ID from the cache directly
- use `@CacheEvict` annotation, see docs [here](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#cache-annotations-evict)

For simplicity sake, I decided to use the second option because at the moment this is more than enough for me, below is how it looks like

```java
@Slf4j
public class MongoUserRepository implements UserRepository {

    @Override
    @CacheEvict(value = CacheConstants.DBREF_CACHE_NAME, key = "#user.id", beforeInvocation = true)
    public User update(User user) {
        // update user in the DB
        return user;
    }
}
```

Hopefully, this information will be useful!
