## Seulement REST ?

-@@-

## les extensions

tonton maven à la rescousse

-@@-

## Liste des extensions

```shell
mvn quarkus:list-extensions
```
```
Current Quarkus extensions available:
Agroal - Database connection pool        quarkus-agroal
...
JDBC Driver - H2                         quarkus-jdbc-h2
JDBC Driver - MariaDB                    quarkus-jdbc-mariadb
JDBC Driver - PostgreSQL                 quarkus-jdbc-postgresql
Jackson                                  quarkus-jackson
```
<!-- .element class="fragment" -->

> Il y en a 55 actuellemnt<!-- .element class="fragment" -->

-@@-

## Ajouter une extention

commande maven : 

mvn quarkus:add-extension
-Dextensions="groupId:artifactId"

-@@-

## Ajouter une extension

```shell
mvn quarkus:add-extension 
  -Dextensions="io.quarkus:quarkus-hibernate-orm"
```

```xml
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-hibernate-orm</artifactId>
</dependency>
```