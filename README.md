== Basic Spring Cloud Source Application

This project contains a Spring Cloud Source applications that publishes a String at a regular interval to initiate the stream. For this example, publish a name as a String.

The example shows how the Spring Cloud Stream concept of a `Source` maps to the logical equivalent of the Java 8 function `Supplier`.

This Source application can be used in conjunction with `Processor` and `Sink` applications to show a full end to end flow.  The `Processor` uses a Java 8 `Function` and the `Sink` uses a Java 8 `Consumer`. 

You can create these additional applications by using the Spring CLI command

```
spring cli new scs-processor
````

and 

```
spring cli new scs-sink
```

== Prerequisites

To use Spring Cloud Stream functionality, we need to ensure that a message broker is accessible. For this guide, we use RabbitMQ. If a local Docker environment is found, the following command can start RabbitMQ:

```bash
docker run -d --hostname my-rabbit --name some-rabbit -p 15672:15672 -p 5672:5672 rabbitmq:3-management
```

The result is that RabbitMQ should be accessible locally with the username/password of guest/guest.


== Building the Source Application

```
./mvnw clean package
```

This runs unit tests, which are a bit advanced to explain in a short getting started guide.
The Spring Cloud Stream reference docs have more information.


== Running the Source Application
```
./mnvw spring-boot:run
```

There are unit test

The Source (`java.util.function.Supplier`) is defined in `NameSourceConfiguration` as:

```java
@Bean
public Supplier<String> supplyName() {
  return () -> "Christopher Pike";
}
```

The name of the source output is defined in `application.properties`

```
spring.cloud.stream.function.bindings.supplyName-out-0=processorinput
```

When using this application in conjunction with the `Processor` sample application, the name used for the output of the `Source` should match the name of the input for the `Processor`.  This is already setup for both applications.


=== Multiple Functions in one applicaiton

More than one Java 8 Function can exist in a single application.  
If you would like to add the `Processor` and `Sink` to the same application as the source, use the following Spring CLI commands.

```
spring cli add scs-processor
````

and 

```
spring cli add scs-sink
```
