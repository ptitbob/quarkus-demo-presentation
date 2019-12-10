### JPA & Hibernate

**JSR 338**<!-- .element style="color: #e57125; float: right; font-size: 80%" -->

-@@-

### Hibernate

> Implementation JPA

-@@-

### Hibernate

La configuration...

... via application.properties

notes:

-@@-

### Hibernate

La configuration...

... via application.properties

```
quarkus.datasource.url=jdbc:postgresql://localhost:5432/postgres
quarkus.datasource.driver=org.postgresql.Driver
quarkus.datasource.username=postgres
quarkus.datasource.password=postgres
```

notes:
Il n'y a pas de META-INF/persistence.xml, quarkus en analysant le fichier de properties identifie le(s) connexion DB
Mais pour des besoins avancés, il est possible de mettre en place ce fichier

-@@-

### Hibernate

*2 pattern possible*

* approche DAO (aka Repository dans un autre monde)<!-- .element class="fragment" -->
* Approche Active record (JPA with panache)<!-- .element class="fragment" -->

-@@-

### Hibernate / DAO

* entité
* DAO/repository (accès aux données)
* service (traitement métier)
* exposition

-@@-

### Hibernate avec Panache

> Là cela commence a devenir fun :)

**Les bonnes idées de Play**

**Et active record**

-@@-

### Hibernate avec Panache

```shell
mvn quarkus:add-extension
  -Dextensions="quarkus-hibernate-orm-panache"
```

-@@-

### Hibernate avec Panache

```xml
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-hibernate-orm-panache</artifactId>
</dependency>
```

-@@-

### Active record

En génie logiciel, le patron de conception (design pattern) active record (enregistrement actif en français) est une approche pour lire les données d'une base de données. Les attributs d'une table ou d'une vue sont encapsulés dans une classe. Ainsi l'objet, instance de la classe, est lié à un tuple de la base. Après l'instanciation d'un objet, un nouveau tuple est ajouté à la base au moment de l'enregistrement. Chaque objet récupère ses données depuis la base ; quand un objet est mis à jour, le tuple auquel il est lié l'est aussi. La classe implémente des accesseurs pour chaque attribut. <!-- .element style="font-size: 60%;" -->

*- Wikipedia*<!-- .element style="color: #e57125; float: right" -->

-@@-

### Active record

### TL;DR

-@@-

### Active record

**L'entité porte les méthodes pour la manipuler**

-@@-

### Hibernate with *Panache*

Les entité herite de `PanacheEntity`

```java
@Entity
public class City extends PanacheEntity {

  @Column(name = "insee")
  public String inseeId;

  @Column(name = "nom_maj")
  public String name;

}
```
-@@-

### Hibernate with *Panache*

*Ou est l'`Id` ?*

-@@-

### Hibernate with *Panache*

![](images/panache/entity.png)

> Il y a un loup !<!-- .element class="fragment" style="color:crimson" -->

-@@-

### Hibernate with *Panache*

`PanacheEntity` est **opinionated**<!-- .element style="color: crimson" --> !

L'id "panache" a besoin d'une sequence hibernate !<!-- .element class="fragment" -->

-@@-

### Hibernate with *Panache*

```sql
create sequence hibernate_sequence;
...
create table person
  (
    id  bigint  not null ,
    ...
  );
```

> A retenir...

-@@-

### Hibernate with *Panache*

Active record, ça donne quoi ?

```java
public Person getPersonById(Long id) {
  return Person.findById()
}
```
ou par inférence de requête
```java
public Person getPersonByLogin(String login) {
  return Person.find("login", login).firstResult();
}
```

-@@-

### Hibernate with *Panache*

Ajout de méthodes "active" record directement sur l'entité

```java
@Entity
public class Person extends PanacheEntity {

  @NotBlank @Column(unique = true)
  public String login;
  ...
  public Boolean active;

  public static List<Person> listByActiveFlag(
      Boolean activeFlag
  ) {
    return find("active", activeFlag).list();
  }
}
```

-@@-

### Hibernate with *Panache*

Possibilité d'utiliser des "repository"

![](images/PanacheRepository.png)

```java
public interface PanacheRepository<Entity> 
  extends PanacheRepositoryBase<Entity, Long> {
}
```

-@@-

### Hibernate with *Panache*

Possibilité d'utiliser des "repository"

```java
@ApplicationScoped
public class PersonRepository 
  implements PanacheRepository<Person> {
  ...
  public Person findByName(String name){
      return find("name", name).firstResult();
  }
  ...
}
```

-@@-

### Hibernate with *Panache*

> Dans le code, cela donne quoi ?