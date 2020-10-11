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

