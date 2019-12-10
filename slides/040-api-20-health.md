### Health

Le carnet de santé de l'application !

Microprofile<!-- .element style="color: #e57125; float: right; font-size: 80%" -->

-@@-

### Health

C'est aussi une extension

```
mvn quarkus:add-extension -Dextension=quarkus-smallrye-health
```

-@@-

### Health

Ajout d'un endpoint

```
http :8080/health
HTTP/1.1 200 OK
content-length: 46
content-type: application/json; charset=UTF-8
{
    "checks": [],
    "status": "UP"
}
```

-@@-

### Health

Endpoint a enrichir

-@@-

### Health

Simple check

```java
@Liveness
@ApplicationScoped
public class GreetingHealth implements HealthCheck {
  @Override
  public HealthCheckResponse call() {
    return HealthCheckResponse.up("Greeting health");
  }
}
```

-@@-

### Health

Supervision d'une API REST appelée
```java
  @Inject
  @RestClient
  HelloHealthRepository helloHealthRepository;

  @Override
  public HealthCheckResponse call() {
    try {
      helloHealthRepository.getHealth();
      return HealthCheckResponse.up("Serveur Hello");
    } catch (Throwable throwable) {
      return HealthCheckResponse.down("Serveur Hello");
    }
  }
```

-@@-

### Health

Et les base de données ?

C'est automatique<!-- .element class="fragment" -->

notes:
Dès qu'une base est connecté, Health le repère 

-@@-

### Health

Et les base de données ?

```
http :8080/health/
HTTP/1.1 200 OK
{
    "checks": [
        ...
        {
            "name": "Database connection(s) health check",
            "status": "UP"
        }
    ],
    "status": "UP"
}
```

notes:
Dès qu'une base est connecté, Health le repère 

-@@-

### Health

Si la base est KO

```
http :8080/health/
HTTP/1.1 503 Service Unavailable
{
  "checks": [
    ...
    {
      "data": {
          "quarkus-default-ds": "Unable t..."
      },
      "name": "Database connection(s) health check",
      "status": "DOWN"
    }
  ],
  "status": "DOWN"
}
```
