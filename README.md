# RabbitMQ Client for Vert.x

A Vert.x client allowing applications to interact with a RabbitMQ broker (AMQP 0.9.1)

**This service is experimental and the APIs are likely to change before settling down.**

# Getting Started

## Maven

Add the following dependency to your maven project

```xml
<dependencies>
  <dependency>
    <groupId>io.vertx</groupId>
    <artifactId>vertx-rabbitmq-client</artifactId>
    <version>$version</version>
  </dependency>
</dependencies>
```

## Gradle ##

Add the following dependency to your gradle project

```groovy
dependencies {
  compile("io.vertx:vertx-rabbitmq-client:$version")
}
```

## Create a client

You can create a client instance as follows:

```java
RabbitMQClient client = RabbitMQClient.create(vertx, config);
```

# Operations

The following are some examples of the operations supported by the RabbitMQService API. Consult the javadoc/documentation
for detailed information on all API methods.

## Publish

Publish a message to a queue

```java
JsonObject message = new JsonObject().put("body", "Hello RabbitMQ, from Vert.x !");
client.basicPublish("", "my.queue", message, pubResult -> {
  if (pubResult.succeeded() {
    System.out.println("Message published !");
  } else {
    pubResult.cause().printStackTrace();
  }
});
```

## Consume

Consume messages from a queue

```java
// Create the event bus handler which messages will be sent to
vertx.eventBus().consumer("my.address", msg -> {
  JsonObject json = (JsonObject) msg.body();
  System.out.println("Got message: " + json.getString("body"));
});

// Setup the link between rabbitmq consumer and event bus address
client.basicConsume("my.queue", "my.address", consumeResult -> {
  if (consumeResult.succeeded()) {
    System.out.println("RabbitMQ consumer created !");
  } else {
    consumeResult.cause().printStackTrace();
  }
});
```

## Get

Will get a message from a queue

```java
client.basicGet("my.queue", true, getResult -> {
  if (getResult.succeeded()) {
    JsonObject msg = getResult.result();
    System.out.println("Got message: " + msg.getString("body"));
  } else {
    getResult.cause().printStackTrace();
  }
});
```

# Running the tests

You will need to have RabbitMQ installed and running with default ports on localhost for this to work.
