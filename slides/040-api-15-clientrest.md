### Client REST

**JSR 370 (v2.1)**<!-- .element style="color: #e57125; float: right; font-size: 80%" -->

-@@-

### Client REST

Appel vers des API REST

C'est simple comme une interface<!-- .element class="fragment" -->

et une extension<!-- .element style="color: black" -->

`mvn quarkus:add-extensions -Dextension=quarkus-rest-client`<!-- .element style="color: black" -->

-@@-

### Client REST

Appel vers des API REST

C'est simple comme une interface

et une extension

`mvn quarkus:add-extensions -Dextension=quarkus-rest-client`

-@@-

### Client REST

```java
@RegisterRestClient(baseUri = "http://localhost:8080")
@Produces({MediaType.TEXT_PLAIN})
@Path("/hello")
@ApplicationScoped
public interface HelloClient {

  @GET
  String hello();

}
```

-@@-

### Client REST

```java
    @Inject
    @RestClient
    HelloClient helloClient;

    @GET
    @Produces(MediaType.TEXT_PLAIN)
    public String hello() {
        return "call --> " + helloClient.hello();
    }
```

-@@-

### Client REST

> C'est un peu trop statique ?

***Configurons !***<!-- .element class="fragment" -->

-@@-

### Client REST

```
org.acme.quickstart.repository
    .HelloRepository/mp-rest/url = http://localhost:8083
org.acme.quickstart.repository
    .HelloRepository/mp-rest/scope=javax.inject.Singleton
```
```java
package org.acme.quickstart.repository;
...
@RegisterRestClient
@Produces({MediaType.TEXT_PLAIN})
@Path("/hello")
public interface HelloRepository {
```
