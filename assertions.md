# Assertions

**RecordoAssertion** verifies that the actual value is equal to the expected one using json assertion. The expected value is loaded from a file.

{% hint style="success" %}
If a file is absent, the actual result will be saved as expected.
{% endhint %}

{% hint style="info" %}
If an assertion fails, the actual object will be saved to a new file for comparison.
{% endhint %}

## Examples

```java
@Test
void should_get_book_by_id() {
    Book actual = ...
    RecordoAssertion.assertAsJson(actual)
            .isEqualTo("/books/book.json");
}
```

```java
@Test
void should_get_books_by_author(
        @Read("/books/author.json") Author author
) {
    Page<Book> actual = bookService.findAllByAuthor(author);
    
    RecordoAssertion.assertAsJson(actual)
            .extensible(true)
            .including("content.id", "content.title", "content.author.id")
            .isEqualTo("/books/short_books.json");
}
```

```java
@Test
void should_get_all_books() {
    List<Book> actual = bookService.findAll();
    
    RecordoAssertion.assertAsJson(actual)
            .excluded("description", "author.comments")
            .withStrictOrder(false)
            .isEqualTo("/books/all_books.json");
}
```

