# HibernateExample

A Java 17 / Spring Boot example that uses Hibernate/JPA to map `Department` and `Employee` entities to relational tables. It includes Spring Data repositories, a REST endpoint for looking up departments, and a standalone Hibernate utility example.

## Requirements

- Java 17
- Maven 3.6 or later
- MySQL, if running with the datasource settings in `src/main/resources/application.properties`

## Database configuration

By default, the Spring Boot application is configured to connect to a MySQL database named `LocalDB` on `127.0.0.1:3306`. Update `src/main/resources/application.properties` to match your local database, or override the Spring datasource settings with environment variables:

```powershell
$env:SPRING_DATASOURCE_URL = "jdbc:mysql://127.0.0.1:3306/LocalDB"
$env:SPRING_DATASOURCE_USERNAME = "your-username"
$env:SPRING_DATASOURCE_PASSWORD = "your-password"
```

The SQL example in `src/main/resources/mysqlscript.sql` creates a `DEPARTMENT` table and inserts sample departments. Run it against `LocalDB` if you want to try the department lookup endpoint.

Do not commit database passwords or other credentials. Keep local secrets out of source control and rotate any credential that has been committed.

## Build and run

From this directory:

```bash
mvn test
mvn spring-boot:run
```

The application starts on port `8080` by default. You can also build a package with `mvn package` and run the generated JAR if the Spring Boot Maven plugin is configured to repackage it.

## API

Fetch a department by ID:

```http
GET http://localhost:8080/data/getDepartments/1
```

The endpoint returns the department name as plain text. If the ID does not exist, the application raises a not-found exception.

## Project layout

- `domain/` - JPA entities for `Department` and `Employee`
- `repository/` - Spring Data JPA repositories
- `controller/` - REST endpoint for department lookup
- `util/HibernateUtil.java` - Standalone Hibernate `SessionFactory` setup
- `src/main/resources/hibernate.cfg.xml` - Standalone Hibernate configuration
- `src/main/resources/mysqlscript.sql` - Example MySQL schema and data
- `src/test/` - Test sources
