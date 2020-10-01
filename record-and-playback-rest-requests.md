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

```java
@Test
@MockHttpServer("/mockhttp/should_retrieve_gists.rest.json")
void should_retrieve_gists() {
    ...
    final List<GistResponse> gists = gitHubClient.getGists();
    ...
}
```

