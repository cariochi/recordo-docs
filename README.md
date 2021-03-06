# What is Recordo

{% hint style="warning" %}
This documentation is still under development, so you can find some missing sections.
{% endhint %}

**Recordo** is a JUnit 5 extension for fast, deterministic, and accurate tests. It implements common test functionality in a declarative way and helps to handle json resources by recording or generating json files if they are absent.

## Features

### [Read Objects From Json Files](features/load-resources.md)

Allows deserializing objects from resources. If a json file is absent, it will be created automatically.

{% tabs %}
{% tab title="Test" %}
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

### [Json Assertions](features/assertions.md)

Allows making Json Assertions. If a json file is absent, the actual value will be saved to file for future assertions.

{% tabs %}
{% tab title="Java" %}
```java
@Test
void should_get_book_by_id() {
    Book actual = ...
    
    RecordoAsserion.assertAsJson(actual)
            .excluding("author.firstName", "author.lastName")
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

### [Record and Playback Mock Server](features/record-and-playback-rest-requests.md)

Simulates HTTP-based APIs. If a json file is absent, all requests will be executed with real servers and all requests and responses will be saved.

{% tabs %}
{% tab title="Java" %}
```java
@Test
@WithMockHttpServer("/mockServer/get_gists.rest.json")
void should_retrieve_gists() {
    ...
    List<GistResponse> gists = restClient.getGists();
    ...
}
```
{% endtab %}

{% tab title="get\_gists.rest.json \(recorded\)" %}
```javascript
[
  {
    "request": {
      "method": "GET",
      "url": "https://api.github.com/gists",
      "headers": {
        "authorization": "********",
        "accept": "application/json, application/*+json"
      },
      "body": null
    },
    "response": {
      "protocol": "HTTP/1.1",
      "statusCode": 200,
      "statusText": "OK",
      "headers": {
        "content-type": "application/json; charset=utf-8"
      },
      "body": [
        {
          "url": "https://api.github.com/gists/77c974bae1167df5c880f4849b7e001c",
          "forks_url": "https://api.github.com/gists/77c974bae1167df5c880f4849b7e001c/forks",
          "commits_url": "https://api.github.com/gists/77c974bae1167df5c880f4849b7e001c/commits",
          "id": "77c974bae1167df5c880f4849b7e001c",
          "node_id": "MDQ6R2lzdDc3Yzk3NGJhZTExNjdkZjVjODgwZjQ4NDliN2UwMDFj",
          "git_pull_url": "https://gist.github.com/77c974bae1167df5c880f4849b7e001c.git",
          "git_push_url": "https://gist.github.com/77c974bae1167df5c880f4849b7e001c.git",
          "html_url": "https://gist.github.com/77c974bae1167df5c880f4849b7e001c",
          "files": {
            "hello_world.txt": {
              "filename": "hello_world.txt",
              "type": "text/plain",
              "language": "Text",
              "raw_url": "https://gist.githubusercontent.com/vadimdeineka/77c974bae1167df5c880f4849b7e001c/raw/d66c8d4d32962340839b015b7849e067d0f79479/hello_world.txt",
              "size": 14
            }
          },
          "public": false,
          "created_at": "2020-06-26T14:19:30Z",
          "updated_at": "2020-06-26T14:19:32Z",
          "description": "Hello World!",
          "comments": 0,
          "user": null,
          "comments_url": "https://api.github.com/gists/77c974bae1167df5c880f4849b7e001c/comments",
          "owner": {
            "login": "vadimdeineka",
            "id": 9740075,
            "node_id": "MDQ6VXNlcjk3NDAwNzU=",
            "avatar_url": "https://avatars3.githubusercontent.com/u/9740075?v=4",
            "gravatar_id": "",
            "url": "https://api.github.com/users/vadimdeineka",
            "html_url": "https://github.com/vadimdeineka",
            "followers_url": "https://api.github.com/users/vadimdeineka/followers",
            "following_url": "https://api.github.com/users/vadimdeineka/following{/other_user}",
            "gists_url": "https://api.github.com/users/vadimdeineka/gists{/gist_id}",
            "starred_url": "https://api.github.com/users/vadimdeineka/starred{/owner}{/repo}",
            "subscriptions_url": "https://api.github.com/users/vadimdeineka/subscriptions",
            "organizations_url": "https://api.github.com/users/vadimdeineka/orgs",
            "repos_url": "https://api.github.com/users/vadimdeineka/repos",
            "events_url": "https://api.github.com/users/vadimdeineka/events{/privacy}",
            "received_events_url": "https://api.github.com/users/vadimdeineka/received_events",
            "type": "User",
            "site_admin": false
          },
          "truncated": false
        }
      ]
    }
  }
]
```
{% endtab %}
{% endtabs %}

### [Mock Client](features/web-layer-testing.md)

A convenient way to test your own end-points. It wraps **Spring MockMvc** and deserializes responses.

{% tabs %}
{% tab title="Execute and get response content" %}
```java
@Test
void should_get_books(
        @MockHttpGet("/books") Page<Book> books
) {
   ...
}
```
{% endtab %}

{% tab title="Execute and get response" %}
```java
@Test
void should_get_books(
        @MockHttpGet("/users/1/books?page=2") Response<Page<Book>> response
) {
   ...
   Page<Book> books = responce.getContent();
   HttpStatus status = response.getStatus()
   Map<String, String> headers = response.getHeaders()
   ...
}
```
{% endtab %}

{% tab title="Prepare request" %}
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
{% endtabs %}

## Concept

The main concept of the **Recordo** extension is that the test will generate or record json resources if they are absent. 

The most common test creation scenario is:

1. You create a test and run it for the first time.
2. **Recordo** extension creates json files. Test fails.
3. You verify and modify json files if necessary.
4. You run a test for the second time. Test passes.

In case you have several test parameters provided by the **Recordo** extension, you may need several test-preparation runs.

## License

**Recordo** extension is licensed under the MIT License. See the [LICENSE](https://github.com/cariochi/recordo/blob/master/LICENSE) file for details.

