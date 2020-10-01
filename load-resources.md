# Load Resources

**Recordo** extension automatically loads resources from json files and provides them as test input parameters.

{% hint style="info" %}
If a json file is absent, a new file with an empty object will be created.
{% endhint %}

## Examples

```java
@Test
void should_create_book(
    @Given("/books/book.json") Book book
) {
    ...
}
```

```java
@Test
void should_create_book(
    @Given("/books/books.json") List<Book> books
) {
    ...
}
```

