# Assertions

**RecordoAssertion** verifies that the actual value is equal to the expected one using [JsonAssert](https://github.com/skyscreamer/JSONassert). The expected value is loaded from a file.

{% hint style="info" %}
* If a file is absent, the actual result will be saved as expected.
* If an assertion fails, the actual object will be saved to a new file for comparison.
{% endhint %}

## Assertion Parameters

|  |  |
| :--- | :--- |
| **using**\(ObjectMapper mapper\) | Specifies an **ObjectMapper** instance that will be used instead of the default one. |
| **extensible**\(boolean value\) | Allows keys in actual that don't appear in expected. Default value is `false`. |
| **withStrictOrder**\(boolean value\) | Requires strict ordering of array elements. Default value is `true`. |
| **including**\(String... fields\) | Defines fields that should be used in the comparison. All other fields will be ignored. Allows nested fields. |
| **excluding**\(String... fields\) | Defines fields that should be excluded from the comparison. Allows nested fields. |

## Examples

{% tabs %}
{% tab title="Java" %}
```java
@Test
void should_get_book_by_id() {
    Book actual = bookService.getById(1L);
    
    RecordoAssertion.assertAsJson(actual)
            .isEqualTo("/books/book.json");
}
```
{% endtab %}

{% tab title="book.json" %}
```javascript
{
  "id": 1,
  "title": "Othello",
  "author": {
    "id": 1,
    "firstName": "William",
    "lastName": "Shakespeare"
  }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Java" %}
```java
@Test
void should_get_books_by_author(
        @Read("/books/author.json") Author author
) {
    Page<Book> actual = bookService.findAllByAuthor(author);
    
    RecordoAssertion.assertAsJson(actual)
            .using(objectMapper)
            .extensible(true)
            .including("content.id", "content.title", "content.author.id")
            .isEqualTo("/books/short_books.json");
}
```
{% endtab %}

{% tab title="author.json" %}
```javascript
{
  "id": 1,
  "firstName": "William",
  "lastName": "Shakespeare"
}
```
{% endtab %}

{% tab title="short\_books.json" %}
```javascript
{
  "content": [
    {
      "id": 1,
      "title": "Othello",
      "author": {
        "id": 1
      }
    },
    {
      "id": 2,
      "title": "Macbeth",
      "author": {
        "id": 1
      }
    },
    {
      "id": 3,
      "title": "Richard II",
      "author": {
        "id": 1
      }
    }
  ]
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Java" %}
```java
@Test
void should_get_all_books() {
    List<Book> actual = bookService.findAll();
    
    RecordoAssertion.assertAsJson(actual)
            .excluded("author.firstName", "author.lastName")
            .withStrictOrder(false)
            .isEqualTo("/books/short_books.json");
}
```
{% endtab %}

{% tab title="short\_books.json" %}
```javascript
{
  "content": [
    {
      "id": 1,
      "title": "Othello",
      "author": {
        "id": 1
      }
    },
    {
      "id": 2,
      "title": "Macbeth",
      "author": {
        "id": 1
      }
    },
    {
      "id": 3,
      "title": "Richard II",
      "author": {
        "id": 1
      }
    }
  ]
}
```
{% endtab %}
{% endtabs %}

