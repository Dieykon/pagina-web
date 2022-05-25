---
title: ⛓ Convert event-based API to Flux
date: 2019-11-16 12:00:00 -0400
categories: [Java, Reactor]
tags:
  - java
  - flux
---

When working with reactive streams in Java, you may encounter a situation when there is an event-based API that must somehow return data as a stream.

In my case for [Reactive Streams](https://github.com/reactive-streams/reactive-streams-jvm), I use [Reactor](https://projectreactor.io/). Let's say we have a simple listener object with `onUpdate` callback method, like in the code snippet below.

```java
public class CustomListener implements BaseListener {
    private final FluxConnector<String[]> namesFluxConnector;;

    public MarketDataListener() {
        namesFluxConnector = new FluxConnector<>();
    }

    @Override
    public void onUpdate(String[] names) {
        namesFluxConnector.next(names);
    }

    public Flux<String[]> getFlux() {
        return namesFluxConnector.getFlux();
    }
}
```

And we need to work with data as with stream, in the code above `CustomListener` returns `Flux<String[]>` object to consume the stream later. The main transformations are done in `FluxConnector`. `FluxConnector` creates a `Flux` and uses `FluxSink` to push objects into stream. A simple implementation to convey the main idea in the code snippet below.

```java
public class FluxConnector<T> {
    private final Flux<T> flux;
    private FluxSink<T> fluxSink;

    public FluxConnector() {
        this.flux = Flux
            .create(new SinkAdapter<T>(this::setFluxSink))
            .share();
    }

    public void next(T data) {
        if (Objects.nonNull(fluxSink))
            fluxSink.next(data);
    }

    public Flux<T> getFlux() {
        return flux;
    }

    private void setFluxSink(FluxSink<T> fluxSink) {
        if (Objects.isNull(this.fluxSink))
            this.fluxSink = fluxSink;
    }

    private static class SinkAdapter<T> implements Consumer<FluxSink<T>> {
        private final Consumer<FluxSink<T>> consumer;

        SinkAdapter(Consumer<FluxSink<T>> consumer) {
            this.consumer = consumer;
        }

        @Override
        public void accept(FluxSink<T> sink) {
            consumer.accept(sink);
        }
    }
}
```

### Links

- [Project Reactor](https://projectreactor.io/)
- [Reactive Streams Specification](https://github.com/reactive-streams/reactive-streams-jvm)
- [Reactive Streams](http://www.reactive-streams.org/)
