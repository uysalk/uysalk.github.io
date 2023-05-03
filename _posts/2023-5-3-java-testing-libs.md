---
layout: post
title: Awesome Java testing libraries
---
As a Java developer, you want to ensure that your code is reliable and performs well. This requires effective unit and integration testing. Fortunately, there are many libraries available that can help you write better tests and catch potential issues before they become problems in production. In this post, I will explore some of the awesome libraries for Java unit and integration testing.

### AssertJ
AssertJ is a powerful assertion library for Java that provides a fluent, readable syntax for testing. It makes it easy to write expressive assertions that clearly describe the expected behavior of your code. AssertJ also provides a wide range of assertion methods that can be used to test everything from primitive types to complex objects.

Example 1: Assert that a string starts with a specific prefix:
```java
String actual = "hello world";
assertThat(actual).startsWith("hello");
```

Example 2: Assert that a collection contains a specific element:
```java
List<Integer> actual = Arrays.asList(1, 2, 3);
assertThat(actual).contains(2);
```


### Awaitility
Awaitility is a library that allows you to write asynchronous tests in a synchronous style. It provides a simple and concise API that allows you to wait for certain conditions to be met before proceeding with the test. This is particularly useful when testing code that makes use of asynchronous APIs or services.
Example 1: Wait for a specific condition to be true:
```java
await().atMost(Duration.ofSeconds(10)).until(() -> list.size() == 5);
```

Example 2: Wait for an exception to be thrown:
```java
await().atMost(Duration.ofSeconds(10)).untilAsserted(() -> assertThatThrownBy(() -> someMethod()).isInstanceOf(SomeException.class));
```
### Mockito
Mockito is a popular mocking framework for Java that allows you to create mock objects and stub their behavior. It simplifies the process of mocking dependencies and allows you to write tests that are isolated from external dependencies.
Example 1: Mocking a dependency and setting up its behavior:
```java
SomeDependency mockDependency = mock(SomeDependency.class);
when(mockDependency.someMethod()).thenReturn("someValue");
```

Example 2: Verifying that a method on a mock was called with specific arguments:
```java
SomeDependency mockDependency = mock(SomeDependency.class);
someService.doSomething(mockDependency);
verify(mockDependency).someMethod("expectedArgument");
```

### Wiser
Wiser is a library that provides a simple way to test email functionality in Java. It provides a mock SMTP server that can be used to test email sending and receiving without the need for an actual email server.
Example 1: Testing email sending:
```java
Wiser wiser = new Wiser();
wiser.start();
someService.sendEmail("to@example.com", "subject", "body");
assertThat(wiser.getMessages()).hasSize(1);
wiser.stop();
```

Example 2: Testing email receiving:
```java
Wiser wiser = new Wiser();
wiser.start();
SmtpMessage message = new SmtpMessage();
message.from("from@example.com").to("to@example.com").subject("subject").body("body");
wiser.deliver(message);
assertThat(wiser.getMessages()).hasSize(1);
wiser.stop();
```

### Memoryfilesystem
Memoryfilesystem is a library that provides an in-memory file system that can be used for testing. It allows you to create files and directories in memory and perform file operations without touching the actual file system.
Example 1: Writing a file to an in-memory file system:
```java
FileSystem fs = MemoryFileSystemBuilder.newEmpty().build();
Path path = fs.getPath("/example.txt");
Files.write(path, "example".getBytes());
```

Example 2: Reading a file from an in-memory file system:
```java
FileSystem fs = MemoryFileSystemBuilder.newEmpty().build();
Path path = fs.getPath("/example.txt");
Files.write(path, "example".getBytes());
String actual = Files.readAllLines(path).get(0);
assertThat(actual).isEqualTo("example");
```

### Wiremock
Wiremock is a library that provides a mock HTTP server that can be used for testing HTTP clients. It allows you to simulate different responses from the server and test how your code reacts to different scenarios.
Example 1: Setting up a mock HTTP server to return a specific response:
```java
WireMockServer wireMockServer = new WireMockServer(options().port(8080));
wireMockServer.stubFor(get(urlEqualTo("/example"))
    .willReturn(aResponse()
        .withStatus(200)
        .withHeader("Content-Type", "application/json")
        .withBody("{\"message\": \"hello\"}")));
wireMockServer.start();
```

Example 2: Verifying that a specific request was made to a mock HTTP server:
```java
WireMockServer wireMockServer = new WireMockServer(options().port(8080));
wireMockServer.stubFor(get(urlEqualTo("/example"))
    .willReturn(aResponse().withStatus(200)));
HttpClient httpClient = HttpClient.newHttpClient();
HttpRequest httpRequest = HttpRequest.newBuilder().uri(URI.create("http://localhost:8080/example")).build();
HttpResponse<String> httpResponse = httpClient.send(httpRequest, HttpResponse.BodyHandlers.ofString());
verify(getRequestedFor(urlEqualTo("/example")));
wireMockServer.stop();
```


### Testcontainers
Testcontainers is a library that allows you to create Docker containers for your tests. This is particularly useful when testing code that relies on external services such as databases or message queues.

Example 1: Creating a Docker container for testing a database:

```java
@ClassRule
public static PostgreSQLContainer postgres = new PostgreSQLContainer()
    .withDatabaseName("test")
    .withUsername("user")
    .withPassword("password");

@Test
public void testDatabase() {
    DataSource dataSource = new DriverManagerDataSource(
        postgres.getJdbcUrl(), postgres.getUsername(), postgres.getPassword());
    // use dataSource to interact with the test database
}
```

Example 2: Setting up and using a Kafka container in a JUnit test:
```java
@ClassRule
public static KafkaContainer kafka = new KafkaContainer(DockerImageName.parse("confluentinc/cp-kafka:5.5.1"));

@Test
public void testKafkaContainer() throws InterruptedException {
    String bootstrapServers = kafka.getBootstrapServers();

    // Use Kafka producer and consumer to test messaging
}
```