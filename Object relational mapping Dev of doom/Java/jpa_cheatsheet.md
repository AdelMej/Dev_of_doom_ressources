# Spring Boot JPA Configuration Cheat Sheet

## application.properties (Common Settings)

``` properties
spring.datasource.url=jdbc:mariadb://localhost:3306/mydb
spring.datasource.username=admin
spring.datasource.password=secret
spring.datasource.driver-class-name=org.mariadb.jdbc.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

spring.jpa.database-platform=org.hibernate.dialect.MariaDBDialect
```

## DDL Auto Values

-   **none** --- do nothing
-   **validate** --- check schema but do not change it
-   **update** --- update schema (safe for dev)
-   **create** --- drop + create each run
-   **create-drop** --- create on start, drop on shutdown

## Useful Hibernate Properties

``` properties
spring.jpa.properties.hibernate.jdbc.lob.non_contextual_creation=true
spring.jpa.properties.hibernate.dialect.storage_engine=innodb
```

## Entity Example

``` java
@Entity
@Table(name = "books")
public class BookEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;
}
```

## Enable SQL Logging (Debug Level)

    logging.level.org.hibernate.SQL=DEBUG
    logging.level.org.hibernate.type.descriptor.sql.BasicBinder=TRACE
