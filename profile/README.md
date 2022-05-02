# Amaya web framework

![Logo](https://github.com/amayaframework/.github/raw/main/images/logo.png)

## What is it?

Amaya is a lightweight and modular asynchronous web framework, delivered in parts in several jar packages.

## What can it do?

By default, any build of the framework created on the basis of the amaya-core-api should be able 
to handle user controllers, support configurability, log events that occur, 
and run a http server with the specified parameters.

## Advantages of the framework

This framework is designed not only taking into account the mistakes already made, 
but also with a fresh look at what some things should be.

<p>Main features:</p>

* Java 11 and 17 support by default
* Using "controllers" as the main interface for user interaction
* There is no binding to any platform, the framework assembly can be based on anything: servlets, 
a separate http server, embedded solutions.
* The entire chain of interactions between the controller and the network are encapsulated in "pipelines", 
configurable chains of sequential actions. This allows you to naturally maintain asynchronous.
* The framework is not a monolithic immutable structure - a specific implementation is built on the basis of 
the supplied core api, and almost everything in it can be changed using plugins, add-ons and user code.
* There is no reflection in runtime - all calls to user methods are made using meta-lambdas or dynamic proxies.
* The required minimum of functionality by default: routing with dynamic filtered path parameters, 
 query parameters processing, cookies processing, etc.

## How to get started

### Requirements
* Java 8+
* Maven/Gradle

### Dependencies

Choose between sun- and tomcat-based builds, or create your own if you need a different technology.
Dependency for the sun build:

```Groovy
implementation group: 'io.github.amayaframework', name: 'amaya-sun', version: '1.0.0'
```

Dependency for the tomcat build:

```Groovy
implementation group: 'io.github.amayaframework', name: 'amaya-tomcat', version: '1.0.0'
```

The current versions can be viewed in the corresponding repositories: [sun](https://github.com/AmayaFramework/amaya-core-sun), [tomcat](https://github.com/AmayaFramework/amaya-core-tomcat)

Add all the necessary dependencies to the build manifest:

```Groovy
dependencies {
    annotationProcessor group: 'org.atteo.classindex', name: 'classindex', version: '3.4'
    implementation group: 'io.github.amayaframework', name: 'amaya-core', version: '1.0.1'
    implementation group: 'io.github.amayaframework', name: 'amaya-tomcat', version: '1.0.0'
}
```

### Creating a server

1) Create a Main class

```Java
public class Server {
    public static void main(String[] args) throws Throwable {
        Amaya<?> amaya = new TomcatBuilder().
                bind(8080).
                build();
        amaya.start();
    }
}
```

2) Create a simple http controller
```Java
@Endpoint
public class MyController {
    @Get
    public HttpResponse get(HttpRequest request) {
        return Responses.ok("Hello, world!");
    }
}
```

3) Start the server and navigate to 127.0.0.1:8080
4) PROFIT!

You can also, for example, greet the postman if you use it to debug the api, or repeat a custom message n times.
```Java
@Endpoint
public class MyController {
    @Get 
    public HttpResponse get(HttpRequest request) {
        return Responses.ok("Hello, world!");
    }

    @Get("/postman")
    public HttpResponse postmanToken(HttpRequest request) {
        StringBuilder answer = new StringBuilder();
        answer.append("Hello, Postman! Your token is ").append(request.getHeader("Postman-Token"));
        return Responses.ok(answer);
    }

    @Get("/repeat/{n:int}/{message}")
    public HttpResponse repeat(HttpRequest request, @Path Integer n, @Path String message) {
        StringBuilder answer = new StringBuilder();
        for (int i = 0; i < n; ++i) {
            answer.append(message);
        }
        return Responses.ok(answer);
    }
}
```

## Useful links
* [Current Task Board](https://github.com/orgs/AmayaFramework/projects/1)
* [Framework Wiki](https://github.com/AmayaFramework/.github/wiki)
* [amaya-core](https://github.com/AmayaFramework/amaya-core)
* [example project](https://github.com/AmayaFramework/example-project)