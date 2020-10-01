# Record and Playback REST Requests

Record and playback all third-party REST requests.

{% hint style="info" %}
If a json file is absent, **Recordo** will record all requests and responses.
{% endhint %}

{% hint style="info" %}
If a json file is present, **Recordo** will mock REST servers and playback all recorded requests and responses. 
{% endhint %}

## Initialization

{% tabs %}
{% tab title="OkHttp" %}
```java
@EnableRecordo
private OkHttpClient client;
```
{% endtab %}

{% tab title="Apache HttpClient" %}
```java
@EnableRecordo
private HttpClient httpClient;
```
{% endtab %}
{% endtabs %}

## Examples

```java
@Test
@MockHttpServer("/mockhttp/should_retrieve_gists.rest.json")
void should_retrieve_gists() {
    ...
    final List<GistResponse> gists = gitHubClient.getGists();
    ...
}
```

