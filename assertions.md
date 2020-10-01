# Assertions

**Recordo** verifies that actual value is equal to expected using json assertion.   
The expected value is loaded from a file.

{% hint style="info" %}
If a file is absent, the actual result will be saved as expected.
{% endhint %}

{% hint style="info" %}
If an assertion fails, the actual object will be saved to a new file for comparison.
{% endhint %}

## Examples

```java
@Test
void should_get_book_by_id(
        @Given("/books/book.json") Assertion<Book> assertion
) {
    final Book actual = bookService.findById(1L);
    assertion.assertAsExpected(actual);
}
```

```java
@Test
void should_get_books_by_author(
        @Given("/books/author.json") Author author,
        @Given("/books/short_books.json") Assertion<Page<Book>> assertion
) {
    Page<Book> actual = bookService.findAllByAuthor(author);
    assertion
            .included("content.id", "content.title", "content.author.id")
            .extensible(true)
            .assertAsExpected(aclual);
}
```

```java
@Test
void should_get_all_books(
        @Given("/books/all_books.json") Assertion<List<Book>> assertion
) {
    List<Book> actual = bookService.findAll();
    assertion
            .excluded("description", "author.comments")
            .strictOrder(false)
            .assertAsExpected(actual);
}
```

