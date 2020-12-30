# Mock Client

**Recordo** extension allows testing a web layer in a declarative way. You need just declare a test parameter with a specific annotation and Recodro will prepare a request or even execute it and will provide a response as a parameter.

{% hint style="success" %}
#### Annotations:

* `@MockHttpRequest`
* `@MockHttpGet`
* `@MockHttpPost`
* `@MockHttpPut`
* `@MockHttpPatch`
* `@MockHttpDelete`
{% endhint %}

## Initialization

### Enable MockMvc

**Recordo** extension uses Spring **`MockMvc`** under the hood. To enable it you should just add the**`@EnableRecordo`** annotation.

```java
@EnableRecordo
@Autowired
private MockMvc mockMvc;
```

### Custom ObjectMapper \(optional\)

To use your custom Jackson **`ObjectMapper`** instance, you should enable it by adding the **`@EnableRecordo`**annotation. 

```java
@EnableRecordo
@Autowired
private ObjectMapper objectMapper;
```

## Examples

{% tabs %}
{% tab title="GET Request" %}
```java
@Test
void should_get_books(
        @MockHttpGet("/users/{userId}/books") Request<Page<Book>> request
) {
   ...
   Response<Page<UserDto>> response = request
                .uriVars(1)
                .param("page", "2")
                .header("Authorization", "...")
                .execute();
                
   Page<Book> books = responce.getContent();
   ...
}
```
{% endtab %}

{% tab title="GET Response" %}
```java
@Test
void should_get_books(
        @MockHttpGet(value = "/users/1/books?page=2", headers = "Authorization=...") 
           Response<Page<Book>> response
) {
   ...
   Page<Book> books = responce.getContent();
   HttpStatus status = response.getStatus()
   Map<String, String> headers = response.getHeaders()
   ...
}
```
{% endtab %}

{% tab title="GET Response Content" %}
```java
@Test
void should_get_books(
        @MockHttpGet(value = "/users/1/books?page=2", headers = "Authorization=...") 
           Page<Book> books
) {
   ...
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="POST Request" %}
```java
@Test
void should_create_book(
        @MockHttpPost("/books") Request<Book> request
) {
    ...
    Book book = ...
    Response<Book> response = request.body(book).execute();
    Book createdBook = response.getContent();
    ...
}
```
{% endtab %}

{% tab title="POST Response" %}
```java
@Test
void should_create_book(
        @MockHttpPost(value = "/books", body = "/new_book.json") 
            Response<Book> response
) {
    ...
    Book createdBook = response.getContent();
    ...
}
```
{% endtab %}

{% tab title="POST Response Content" %}
```java
@Test
void should_create_book(
        @MockHttpPost(value = "/books", body = "/new_book.json") Book createdBook
) {
    ...
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="PUT Request" %}
```java
@Test
void should_update_book(
        @MockHttpPut("/books/{id}") Request<Book> request
) {
    ...
    Book book = ...
    Response<Book> response = request.uriVars(1).body(book).execute();
    Book updatedBook = response.getContent();
    ...
}
```
{% endtab %}

{% tab title="PUT Response" %}
```java
@Test
void should_create_book(
        @MockHttpPut(value = "/books/1", body = "/updated_book.json") Response<Book> response
) {
    ...
    Book updatedBook = response.getContent();
    ...
}
```
{% endtab %}

{% tab title="PUT Response Content" %}
```java
@Test
void should_create_book(
        @MockHttpPut(value = "/books", body = "/updated_book.json") 
                Book updatedBook
) {
    ...
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="DELETE Request" %}
```java
@Test
void should_delete_book(
        @MockHttpDelete("/books/{id}") Request<Void> request
) {
    ...
    Response<Void> response = request
        .uriVars(1)
        .expectedStatus(HttpStatus.NO_CONTENT)
        .execute();
    ...
}
```
{% endtab %}

{% tab title="DELETE Response" %}
```java
@Test
void should_delete_book(
        @MockHttpDelete(value = "/books/{id}", expectedStatus = HttpStatus.NO_CONTENT) 
            Response<Void> response
) {
    ...
}
```
{% endtab %}
{% endtabs %}

