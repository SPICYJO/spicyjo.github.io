---
layout: post
title: Spring Boot's auto-configuration of ObjectMapper
description: Let's take a look at auto-configuration of ObjectMapper in Spring Boot
author: Seungwoo Jo
last_modified_at: 2022-01-03 22:53:00 +0900
# math: false
tags: Spring Java Jackson
category: Spring
comments: true
---

## Spring Boot's auto-configured ObjectMapper

As you know, Spring Boot configures a lot of things for us. They register lots of Beans on behalf of us. Sometimes, we might think that there is no configuration at all in the first place.

I think many developers would not pay much attention to serializer and deserializer, because Spring Boot's default configuration works great for most of cases. When we return object (e.g. `Person` object) from `@RestController` controller, `ObjectMapper` serialize the object into Json format behind the scene. `ObjectMapper` also deserialize a Json request body into an object.

When we use Spring Boot, our `ObjectMapper` is little different from Jackson's default one, which is created by `new ObjectMapper()`. Spring Boot's auto-configured `ObjectMapper` has following differences from `new ObjectMapper()`. 

### Spring Boot's auto-configured ObjectMapper
- `SerializationFeature.WRITE_DATES_AS_TIMESTAMPS` is set to `false`
- `SerializationFeature.WRITE_DURATIONS_AS_TIMESTAMPS` is set to `false`
- `DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES` is set to `false`
- `MapperFeature.DEFAULT_VIEW_INCLUSION` is set to `false`


Let's look into what these features do in simple test code. If you want to get test code, you can find it on my github. https://github.com/SPICYJO/blog-demo-1

## Test Class
```java
@Slf4j
@SpringBootTest
class ObjectMapperTest {
    @Autowired
    ObjectMapper springObjectMapper; // Spring Boot auto-configured ObjectMapper

    ObjectMapper jacksonObjectMapper = new ObjectMapper(); // Jackson default ObjectMapper
    
    @BeforeEach
    void setup() {
        jacksonObjectMapper = new ObjectMapper();
    }
    
    /* ... */
}
```

Our test code looks like above. ObjectMapper `springObjectMapper` is autowired from Spring Boot auto-configured Bean and `jacksonObjectMapper` is contructed with `new ObjectMapper()` constructor.

## WRITE_DATES_AS_TIMESTAMPS and WRITE_DURATIONS_AS_TIMESTAMPS

```java
@Test
void test_WRITE_DATES_AS_TIMESTAMPS_WRITE_DURATIONS_AS_TIMESTAMPS() throws JsonProcessingException {
    String springSerialized = springObjectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(
        new TimeClass(1, new Date(), Instant.now(), Duration.ofMinutes(10L))
    );
    log.info(springSerialized);
    //{
    //  "id" : 1,
    //  "date" : "2022-01-03T12:52:11.802+00:00",
    //  "instant" : "2022-01-03T12:52:11.802831Z",
    //  "duration" : "PT10M"
    //}
    jacksonObjectMapper.registerModule(new JavaTimeModule()); // For serialization of Instant, Duration
    String jacksonSerialized = jacksonObjectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(
        new TimeClass(1, new Date(), Instant.now(), Duration.ofMinutes(10L))
    );
    log.info(jacksonSerialized);
    //{
    //  "id" : 1,
    //  "date" : 1641214331840,
    //  "instant" : 1641214331.840827200,
    //  "duration" : 600.000000000
    //}
}
```

You can see that Spring Boot's auto-configured ObjectMapper returned `yyyy-MM-dd'T'HH:mm:ss.SSS` format for `Date` and `Instant` class while Jackson's default mapper returned [unix time](https://en.wikipedia.org/wiki/Unix_time). You can also notice that `Duration` class is formatted in [ISO8601](https://en.wikipedia.org/wiki/ISO_8601) duration format.

## FAIL_ON_UNKNOWN_PROPERTIES

```java
@AllArgsConstructor
@NoArgsConstructor
@Data
public static class Person {
    private Integer id;
    private String name;
}
@Test
void test_FAIL_ON_UNKNOWN_PROPERTIES() throws JsonProcessingException {
    String jsonString = "{\"id\": 42, \"name\": \"John Doe\", \"salary\": 123456}";
    Person springDeserialized = springObjectMapper.readValue(jsonString, Person.class);
    log.info(springDeserialized.toString());
    // ObjectMapperTest.Person(id=42, name=John Doe)

    Person jacksonDeserialized = jacksonObjectMapper.readValue(jsonString, Person.class);
    // above statement throws UnrecognizedPropertyException
    //com.fasterxml.jackson.databind.exc.UnrecognizedPropertyException: Unrecognized field "salary" (class com.example.demo.ObjectMapperTest$Person), not marked as ignorable (2 known properties: "id", "name"])
    // at [Source: (String)"{"id": 42, "name": "John Doe", "salary": 123456}"; line: 1, column: 48] (through reference chain: com.example.demo.ObjectMapperTest$Person["salary"])
    log.info(jacksonDeserialized.toString());
}
```

You can see that Jackson's default mapper throws exception because there was unknown property `salary` in the string. 

## DEFAULT_VIEW_INCLUSION
```java
public static class Views {
    public static class Public {
    }
    public static class Internal {
    }
}

@AllArgsConstructor
@NoArgsConstructor
@Data
public static class Person2 {
    @JsonView({Views.Internal.class})
    private Integer id;
    @JsonView({Views.Public.class, Views.Internal.class})
    private String name;

    private String middleName;
}

@Test
void test_DEFAULT_VIEW_INCLUSION() throws JsonProcessingException, IOException {
    Person2 p = new Person2(42, "John Doe", "Middle");
    String springSerialized = springObjectMapper.writerWithView(Views.Public.class).writeValueAsString(p);
    String jacksonSerialized = jacksonObjectMapper.writerWithView(Views.Public.class).writeValueAsString(p);
    log.info(springSerialized); // {"name":"John Doe"}
    log.info(jacksonSerialized); // {"name":"John Doe","middleName":"Middle"}

    Person2 sp = springObjectMapper.readerWithView(Views.Public.class).readValue("{\"name\":\"John Doe\",\"middleName\":\"Middle\"}", Person2.class);
    Person2 jp = jacksonObjectMapper.readerWithView(Views.Public.class).readValue("{\"name\":\"John Doe\",\"middleName\":\"Middle\"}", Person2.class);
    log.info(sp.toString()); // ObjectMapperTest.Person2(id=null, name=John Doe, middleName=null)
    log.info(jp.toString()); // ObjectMapperTest.Person2(id=null, name=John Doe, middleName=Middle)
}
```

You can see that `middleName` property which has no `@JsonView` annotation is excluded in Spring Boot's auto-configured mapper becuase `MapperFeature.DEFAULT_VIEW_INCLUSION` is set to `false`. 

## Wrap up
In this article, we have seen configuration difference between two `ObjectMapper` from Spring Boot auto-configuration and Jackson default constructor. If you find out more differece, please feel free to leave a comment below. Have a good day!