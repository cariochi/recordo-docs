# Read Objects From Json Files

**Recordo** extension automatically loads resources from json files and provides them as test input parameters.

{% hint style="info" %}
If a json file is absent, a new file with an empty object will be created.
{% endhint %}

## Custom ObjectMapper \(optional\)

To use your custom Jackson **`ObjectMapper`** for json deserialization instead of the default one, you should enable it by adding the **`@EnableRecordo`**annotation to the field. 

```java
@EnableRecordo
@Autowired
private ObjectMapper objectMapper;
```

## Examples

{% tabs %}
{% tab title="Java" %}
```java
@Test
void should_create_book(
    @Read("/books/book.json") Book book
) {
    ...
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
void should_create_book(
    @Read("/books/books.json") List<Book> books
) {
    ...
}
```
{% endtab %}

{% tab title="books.json" %}
```javascript
[
  {
    "id": 1,
    "title": "Othello",
    "author": {
      "id": 1,
      "firstName": "William",
      "lastName": "Shakespeare"
    }
  },
  {
    "id": 2,
    "title": "A Midsummer Night's Dream",
    "author": {
      "id": 1,
      "firstName": "William",
      "lastName": "Shakespeare"
    }
  }
]
```
{% endtab %}
{% endtabs %}

