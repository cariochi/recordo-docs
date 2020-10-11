# Record and Playback REST Requests

Record and playback all third-party REST requests.

{% hint style="info" %}
If a json file is absent, **Recordo** extension will record all requests and responses.
{% endhint %}

{% hint style="info" %}
If a json file is present, **Recordo** extension will mock REST servers and playback all recorded requests and responses. 
{% endhint %}

## Initialization

To record and replay REST requests and responses **Recordo** extension should add an interceptor to your HTTP client.   
Currently, two HTTP clients are supported:

* OkHttp Client
* Apache Http Client

 To enable **Recordo** for an HTTP client you just need to add **`@EnableRecordo`** annotation. 

{% tabs %}
{% tab title="OkHttp" %}
```java
@EnableRecordo
@Autowired
private OkHttpClient okHttpClient;
```
{% endtab %}

{% tab title="Apache HttpClient" %}
```java
@EnableRecordo
@Autowired
private HttpClient httpClient;
```
{% endtab %}
{% endtabs %}

### RestTemplate definition example

{% tabs %}
{% tab title="OkHttp" %}
```java
@Bean
public RestTemplate restTemplate(OkHttpClient okHttpClient) {
    return new RestTemplate(
        new OkHttp3ClientHttpRequestFactory(okHttpClient)
    );
}
```
{% endtab %}

{% tab title="Apache HttpClient" %}
```java
@Bean
public RestTemplate restTemplate(HttpClient httpClient) {
    return new RestTemplate(
        new HttpComponentsClientHttpRequestFactory(httpClient)
    );
}
```
{% endtab %}
{% endtabs %}

### Feign Client definition example

{% tabs %}
{% tab title="OkHttp" %}
```java
@Bean
public feign.Client feignClient(OkHttpClient okHttpClient) {
    return new feign.okhttp.OkHttpClient(okHttpClient);
}
```
{% endtab %}

{% tab title="Apache HttpClient" %}
```java
@Bean
public feign.Client feignClient(HttpClient httpClient) {
    return new feign.httpclient.ApacheHttpClient(httpClient);
}
```
{% endtab %}
{% endtabs %}

## Examples

{% tabs %}
{% tab title="Java" %}
```java
@Test
@WithMockHttpServer("/mockServer/get_gists.rest.json")
void should_retrieve_gists() {
    ...
    List<GistResponse> gists = gitHubClient.getGists();
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

{% tabs %}
{% tab title="Java" %}
```java
@Test
@WithMockHttpServer("/mockhttp/create_and_delete_gist.rest.json")
void should_create_and_delete_gist() {
    ...
    GistResponse response = gitHubClient.createGist(gist);
    Gist created = gitHubClient.getGist(response.getId());
    gitHubClient.deleteGist(response.getId());
    ...
}
```
{% endtab %}

{% tab title="create\_and\_delete\_gist.rest.json \(recorded\)" %}
```javascript
[
  {
    "request": {
      "method": "POST",
      "url": "https://api.github.com/gists",
      "headers": {
        "authorization": "********",
        "content-type": "application/json",
        "accept": "application/json, application/*+json"
      },
      "body": {
        "description": "Hello World!",
        "files": {
          "hello_world.txt": {
            "content": "Hello \nWorld\n!"
          }
        }
      }
    },
    "response": {
      "protocol": "HTTP/1.1",
      "statusCode": 201,
      "statusText": "Created",
      "headers": {
        "content-type": "application/json; charset=utf-8",
        "location": "https://api.github.com/gists/4c16188cd8b6bc2de6ea2eec953ed7ca"
      },
      "body": {
        "url": "https://api.github.com/gists/4c16188cd8b6bc2de6ea2eec953ed7ca",
        "forks_url": "https://api.github.com/gists/4c16188cd8b6bc2de6ea2eec953ed7ca/forks",
        "commits_url": "https://api.github.com/gists/4c16188cd8b6bc2de6ea2eec953ed7ca/commits",
        "id": "4c16188cd8b6bc2de6ea2eec953ed7ca",
        "node_id": "MDQ6R2lzdDRjMTYxODhjZDhiNmJjMmRlNmVhMmVlYzk1M2VkN2Nh",
        "git_pull_url": "https://gist.github.com/4c16188cd8b6bc2de6ea2eec953ed7ca.git",
        "git_push_url": "https://gist.github.com/4c16188cd8b6bc2de6ea2eec953ed7ca.git",
        "html_url": "https://gist.github.com/4c16188cd8b6bc2de6ea2eec953ed7ca",
        "files": {
          "hello_world.txt": {
            "filename": "hello_world.txt",
            "type": "text/plain",
            "language": "Text",
            "raw_url": "https://gist.githubusercontent.com/vadimdeineka/4c16188cd8b6bc2de6ea2eec953ed7ca/raw/d66c8d4d32962340839b015b7849e067d0f79479/hello_world.txt",
            "size": 14,
            "truncated": false,
            "content": "Hello \nWorld\n!"
          }
        },
        "public": false,
        "created_at": "2020-07-06T08:34:56Z",
        "updated_at": "2020-07-06T08:34:56Z",
        "description": "Hello World!",
        "comments": 0,
        "user": null,
        "comments_url": "https://api.github.com/gists/4c16188cd8b6bc2de6ea2eec953ed7ca/comments",
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
        "forks": [],
        "history": [],
        "truncated": false
      }
    }
  },
  {
    "request": {
      "method": "GET",
      "url": "https://api.github.com/gists/4c16188cd8b6bc2de6ea2eec953ed7ca",
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
      "body": {
        "url": "https://api.github.com/gists/4c16188cd8b6bc2de6ea2eec953ed7ca",
        "forks_url": "https://api.github.com/gists/4c16188cd8b6bc2de6ea2eec953ed7ca/forks",
        "commits_url": "https://api.github.com/gists/4c16188cd8b6bc2de6ea2eec953ed7ca/commits",
        "id": "4c16188cd8b6bc2de6ea2eec953ed7ca",
        "node_id": "MDQ6R2lzdDRjMTYxODhjZDhiNmJjMmRlNmVhMmVlYzk1M2VkN2Nh",
        "git_pull_url": "https://gist.github.com/4c16188cd8b6bc2de6ea2eec953ed7ca.git",
        "git_push_url": "https://gist.github.com/4c16188cd8b6bc2de6ea2eec953ed7ca.git",
        "html_url": "https://gist.github.com/4c16188cd8b6bc2de6ea2eec953ed7ca",
        "files": {
          "hello_world.txt": {
            "filename": "hello_world.txt",
            "type": "text/plain",
            "language": "Text",
            "raw_url": "https://gist.githubusercontent.com/vadimdeineka/4c16188cd8b6bc2de6ea2eec953ed7ca/raw/d66c8d4d32962340839b015b7849e067d0f79479/hello_world.txt",
            "size": 14,
            "truncated": false,
            "content": "Hello \nWorld\n!"
          }
        },
        "public": false,
        "created_at": "2020-07-06T08:34:56Z",
        "updated_at": "2020-07-06T08:34:56Z",
        "description": "Hello World!",
        "comments": 0,
        "user": null,
        "comments_url": "https://api.github.com/gists/4c16188cd8b6bc2de6ea2eec953ed7ca/comments",
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
        "forks": [],
        "history": [],
        "truncated": false
      }
    }
  },
  {
    "request": {
      "method": "DELETE",
      "url": "https://api.github.com/gists/4c16188cd8b6bc2de6ea2eec953ed7ca",
      "headers": {
        "authorization": "********",
        "accept": "application/json, application/*+json"
      },
      "body": null
    },
    "response": {
      "protocol": "HTTP/1.1",
      "statusCode": 204,
      "statusText": "No Content",
      "headers": {},
      "body": null
    }
  }
]

```
{% endtab %}
{% endtabs %}

