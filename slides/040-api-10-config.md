### La configuration

**JSR 382**<!-- .element style="color: #e57125; float: right; font-size: 80%" -->

-@@-

### La configuration

1 fichier

**`application.properties`**

-@@-

### La configuration

Le plus simple, changer le port de l'application

```
quarkus.http.port=8083
```

-@@-

### La configuration - API

```
greeting.default.message=Salut
```

```java
@ConfigProperty(name = "greeting.default.message")
private String greetingIntro;
```

-@@-

### La configuration - API

```java
@ConfigProperty(name = "greeting.message.ponctuation")
private Optional<String> ponctuation;
```

*Gestion de la presence ou non de la configuration*

```java
String.format(
    "%s %s%s", 
    greetingIntro, name, ponctuation.orElse(".")
);
```

-@@-

### Les profils Quarkus

3 profils de base

* `dev` actif en developpement (`mvn quarkus:dev`)
* `test` actif en test
* `prod` le profil par défaut

```
greeting.default.message=Salut
%dev.greeting.default.message=Salut (dev)
%test.greeting.default.message=Salut (test)
```
<!-- .element class="fragment" -->


-@@-

### Les profils Quarkus & config

*Les custom*

```
greeting.default.message=Salut
%dev.greeting.default.message=Salut (dev)
%sihm.greeting.default.message=Salut (SIHM)
```
Activation
```
mvn quarkus:dev -Dquarkus-profile=sihm 
```
