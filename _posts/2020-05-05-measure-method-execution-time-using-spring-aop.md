---
title: 📏 Measure method execution time using Spring AOP
date: 2020-05-05 11:00:00 -0400
categories: [Java, Spring]
tags:
  - java
  - spring
---

There are times when it is necessary to be able to log the execution time of a method. The simplest way to achieve that would be to modify every method by adding `Stopwatch` or `System.currentTimeMillis()` at the beginning and at the end of the method. But it leads us to the following inconvenience:

1. the same code should be repeated many times
2. there is no easy way to turn on and off that

But there is a more elegant way to achieve that, keeping measuring code in one single place without the method's body modification. And the answer is AOP.

> Aspect-oriented Programming (AOP) complements Object-oriented Programming (OOP) by providing another way of thinking about program structure. The key unit of modularity in OOP is the class, whereas in AOP the unit of modularity is the aspect. Aspects enable the modularization of concerns (such as transaction management) that cut across multiple types and objects. (Such concerns are often termed “crosscutting” concerns in AOP literature.)

I'm going to use [Spring AOP](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#aop) to measure the execution time of a method.

## Dependency

First of all `spring-boot-starter-aop` dependency needs to be added. Below is an example of the [Maven](http://maven.apache.org/) configuration.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

## Annotation

The next step is to create a custom annotation that will be used to annotate a method that needs to be measured.

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Measured {
    String message() default "";
}
```

There is a message in the annotation to pass a custom message to the log.

## Aspect Class

The next step is to create an [aspect](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#aop-at-aspectj) that will use the `Measured` annotation above.

```java
@Log4j2
@Aspect
@Configuration
@EnableAspectJAutoProxy
public class MeasuredAspect {
    @Around("@annotation(net.fisenko.annotations.Measured)")
    public Object measureExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object proceed = joinPoint.proceed();
        long executionTime = System.currentTimeMillis() - start;
        MethodSignature methodSignature = (MethodSignature) joinPoint.getSignature();
        Method method = methodSignature.getMethod();
        Measured measured = method.getAnnotation(Measured.class);
        String message = measured.message();
        if (Strings.isNullOrEmpty(message))
            log.debug("Method {} execution: {} ms", joinPoint.getSignature().toShortString(), executionTime);
        else
            log.debug("{}: {} ms", message, executionTime);
        return proceed;
    }
}
```

What this code does:

- _`@Aspect`_ annotation declares that this class is an aspect
- `@Around` &nbsp;annotation declares that the [advice](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#aop-advice) will be run before and after the target method
- `@annotation(net.fisenko.annotations.Measured)` is a [pointcut](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#aop-pointcuts), which means that all methods annotated with `@Measured` will be associated with this advice
- The advice itself simply measures time before and after target method execution _`joinPoint.proceed()`_ and logs

## Annotation Usage

Having the annotation and the aspect class the measurement becomes as simple as possible. For example:

```java
@RestController
@RequestMapping("/fibonacci")
public class FibonacciController {
    private final FibonacciService fibonacciService;

    public FibonacciController(FibonacciService fibonacciService) {
        this.fibonacciService = fibonacciService;
    }

    @GetMapping
    @Measured(message = "Get Fibonacci number")
    public ResponseEntity<Long> getFibonacciNumber(@PathVariable int n) {
        return ResponseEntity.ok(fibonacciService.getFibonacciNumber(n));
    }
}
```

## Links

- [https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#aop](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#aop)
- [https://martinfowler.com/articles/domain-oriented-observability.html#Aspect-orientedProgramming](https://martinfowler.com/articles/domain-oriented-observability.html#Aspect-orientedProgramming)
- [https://en.wikipedia.org/wiki/Aspect-oriented_programming](https://en.wikipedia.org/wiki/Aspect-oriented_programming)
