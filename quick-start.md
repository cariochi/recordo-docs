# Quick Start

### Import maven dependency

```markup
<dependency>
    <groupId>com.cariochi</groupId>
    <artifactId>recordo</artifactId>
    <version>1.1.9</version>
    <scope>test</scope>
</dependency>
```

### Extend a test with Recordo extension

```java
@ExtendWith(RecordoExtension.class)
class BookServiceTest {
    ...
}
```

### Enable ObjectMapper to be used by Recordo \(optional\)

If you want your Jackson **ObjectMapper** to be used for json serialization/deserialization for all **Recordo** extension features, you can enable it by the **`@EnableRecordo`**annotation.   
Otherwise, the default **ObjectMapper** will be used.

```java
@EnableRecordo
@Autowired
private ObjectMapper objectMapper;
```

