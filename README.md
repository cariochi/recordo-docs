# What is Recordo

{% hint style="warning" %}
This documentation is still under development, so you can find some missing sections.
{% endhint %}

**Recordo** is a JUnit 5 extension for fast, deterministic, and accurate tests. It implements common test functionality in a declarative way and helps to handle json resources by recording or generating json files if they are absent.

## Features

### Load Resources

{% tabs %}
{% tab title="Test" %}
```java
@Test
void should_create_book(
    @Given("/books/book.json") Book book
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

### Make Assertions

{% tabs %}
{% tab title="Java" %}
```java
@Test
void should_get_book_by_id(
        @Given("/books/book.json") Assertion<Book> assertion
) {
    Book actual = ...
    assertion.assertAsExpected(actual);
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

### Record and Playback  REST Requests

{% tabs %}
{% tab title="Java" %}
```java
@Test
@MockHttpServer("/mockServer/get_gists.rest.json")
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

### Test a Web Layer

```java
@Test
void should_get_books(
        @MockHttpGet("/books") Page<Book> books
) {
   ...
}
```

