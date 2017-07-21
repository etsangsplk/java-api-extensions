[![Build Status][ci-img]][ci] [![Released Version][maven-img]][maven]

# OpenTracing API Extensions

This project provides API extensions to the core OpenTracing APIs.

## APIs

The APIs can be included using the following:

```xml
<dependency>
  <groupId>io.opentracing.contrib</groupId>
  <artifactId>opentracing-api-extensions</artifactId>
</dependency>

```

### Observer

The Observer API can be used to monitor Span related activity and perform additional
tasks.

There are two types of Observer, stateful and stateless.

* A stateful observer will use
identity information supplied with the `SpanData`, to maintain state information about a particular
`Span` instance - and at the appropriate time perform some action using the accumulated information.

* A stateless observer will not maintain any local state information about the `Span` instances, and
instead perform tasks directly in the callback that provides the event/information of interest (e.g. recording
metrics `onFinish` or logging events when `onLog` is called).

The benefit of a stateless approach is that the same observer instance (i.e. singleton) can be used for
all `Span` instances. Whereas the stateful approach will require an observer instance to be instantiated
for each call to `TracerObserver.onStart()`.


## Registering API extensions

There are two ways an extension API can be registered for use with a `Tracer`.

1) Native support within the `Tracer` implementation

Some `Tracer` implementations may decide to implement support for the extension APIs directly, in which
case an implementation specific mechanism will be provided for registering the APIs.

2) Using the extension `Tracer` wrapper

```xml
<dependency>
  <groupId>io.opentracing.contrib</groupId>
  <artifactId>opentracing-api-extensions-tracer</artifactId>
</dependency>

```

The `io.opentracing.contrib.api.tracer.APIExtensionsTracer` provides a single constructor which is supplied
the `Tracer` instance to be wrapped.

This class also provides `addTracerObserver` and `removeTracerObserver` methods to enable a `TracerObserver`
instance to be registered with the tracer wrapper, and perform relevant tasks when new spans are started.


## Release
Follow instructions in [RELEASE](RELEASE.md)

   [ci-img]: https://travis-ci.org/opentracing-contrib/java-api-extensions.svg?branch=master
   [ci]: https://travis-ci.org/opentracing-contrib/java-api-extensions
   [maven-img]: https://img.shields.io/maven-central/v/io.opentracing.contrib/opentracing-api-extensions.svg?maxAge=2592000
   [maven]: http://search.maven.org/#search%7Cga%7C1%7Copentracing-api-extensions
