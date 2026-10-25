# AutomationTesting Project Documentation

## 📋 Project Overview

The **AutomationTesting** project is a comprehensive REST API testing framework that combines:
- A lightweight **REST API server** built with Spring Boot
- An automated **test suite** using TestNG and REST Assured for API testing
- **Data-driven testing** capabilities with JSON-based test data providers

### Purpose
This project serves as both a sample REST API application and a complete automation testing solution for validating REST endpoints. It demonstrates best practices in:
- Building REST APIs with Spring Boot
- Securing APIs with Spring Security
- Automated API testing using TestNG and REST Assured
- Data-driven testing with parameterized test cases
- Parallel test execution

---

## 🏗️ Architecture

### High-Level Architecture
```
┌──────────────────────────────────────┐
│      AutomationTesting Project        │
├──────────────────────────────────────┤
│                                      │
│  ┌─────────────────────────────────┐ │
│  │  REST API Server (Spring Boot)  │ │
│  │  - Application.java             │ │
│  │  - RestAPIController.java       │ │
│  │  - Contact Model                │ │
│  │  - Security Config              │ │
│  └─────────────────────────────────┘ │
│                                      │
│  ┌─────────────────────────────────┐ │
│  │  Test Automation Framework      │ │
│  │  - RestAPITest.java (TestNG)    │ │
│  │  - SalesForceDataProvider.java  │ │
│  │  - Test Data (JSON)             │ │
│  │  - REST Assured                 │ │
│  └─────────────────────────────────┘ │
│                                      │
└──────────────────────────────────────┘
```

### Component Overview

| Component | Purpose | Technology |
|-----------|---------|-----------|
| **Application** | Spring Boot entry point | Spring Boot 3.1.4 |
| **RestAPIController** | Handles REST endpoints | Spring Web |
| **Contact Model** | Data transfer object | Lombok |
| **SecurityConfig** | Security rules & CORS | Spring Security |
| **RestAPITest** | Test cases for APIs | TestNG |
| **SalesForceDataProvider** | Parameterized test data | Jackson, TestNG |

---

## 🛠️ Technology Stack

### Core Technologies
| Technology | Version | Purpose |
|-----------|---------|---------|
| Java | 17 | Programming Language |
| Spring Boot | 3.1.4 | REST API Framework |
| Spring Security | 3.1.4 | Security Framework |
| TestNG | 7.8.0 | Test Framework |
| REST Assured | 5.3.2 | API Testing Library |
| Jackson | 2.x | JSON Processing |
| Lombok | 1.18.24 | Boilerplate Reduction |
| Springdoc OpenAPI | 2.2.0 | Swagger/API Documentation |
| Log4j2 | 2.x | Logging |

### Build Tool
- **Maven** - Dependency management and build automation

---

## 📁 Project Structure

```
AutomationTesting/
├── pom.xml                                    # Maven configuration
├── src/
│   ├── main/
│   │   ├── java/com/restapitest/
│   │   │   ├── Application.java              # Spring Boot entry point
│   │   │   ├── controller/
│   │   │   │   └── RestAPIController.java    # REST endpoints
│   │   │   ├── model/
│   │   │   │   └── Contact.java              # Contact data model
│   │   │   └── config/
│   │   │       ├── SecurityConfig.java       # Security configuration
│   │   │       └── SwaggerConfig.java        # Swagger documentation config
│   │   └── resources/
│   │       ├── application.properties        # App configuration
│   │       └── application.yml               # YAML configuration
│   │
│   └── test/
│       ├── java/com/restapitest/
│       │   ├── RestAPITest.java              # TestNG test cases
│       │   ├── dataprovider/
│       │   │   └── SalesForceDataProvider.java # Data provider for tests
│       │   └── reports/
│       │       └── CustomReporter.java       # Custom test reporting
│       │
│       └── resources/
│           └── json/salesforce/              # Test data (JSON files)
│               ├── contact1.json
│               ├── contact2.json
│               └── ...
│
├── target/                                   # Build output directory
└── AutomationTesting.md                      # This documentation
```

---

## 🚀 Getting Started

### Prerequisites
- **Java 17** or higher installed
- **Maven 3.6.0** or higher installed
- **Git** (optional, for version control)
- IDE (IntelliJ IDEA, Eclipse, or VS Code with Java extensions)

### Installation & Setup

1. **Clone the Repository**
   ```bash
   git clone <repository-url>
   cd AutomationTesting
   ```

2. **Build the Project**
   ```bash
   mvn clean install
   ```
   This command will:
    - Clean previous build artifacts
    - Download all dependencies
    - Compile the source code
    - Run tests (if configured)
    - Create JAR artifact

3. **View Build Output**
   ```bash
   ls target/
   ```

---

## ▶️ Running the Application

### Start the REST API Server

**Option 1: Maven**
```bash
mvn spring-boot:run
```

**Option 2: Run JAR directly**
```bash
mvn clean install
java -jar target/restapi-test-service-0.0.1-SNAPSHOT.jar
```

**Option 3: IDE**
- Right-click `Application.java` → Run as Java Application (IntelliJ)
- Run → Run 'Application.main()' (Eclipse/VS Code)

### Verify Server is Running
The server starts on **`http://localhost:8080`** by default.

Check the status:
```bash
curl http://localhost:8080/items
```

Expected response:
```
"Displaying Controller Success"
```

---

## 🧪 Running the Tests

### Prerequisites for Testing
1. The REST API server must be running on `localhost:8080`
2. Test data JSON files must exist in `src/test/resources/json/salesforce/`

### Run All Tests
```bash
mvn test
```

### Run Specific Test Class
```bash
mvn -Dtest=RestAPITest test
```

### Run Tests with TestNG (Alternative)
```bash
mvn -Dtest=com.restapitest.RestAPITest test
```

### Run Tests in Parallel
TestNG supports parallel execution. Configure in `testng.xml`:
```xml
<suite name="API Tests" parallel="methods" thread-count="5">
    <test name="REST API Tests">
        <classes>
            <class name="com.restapitest.RestAPITest" />
        </classes>
    </test>
</suite>
```

Then run:
```bash
mvn test -DsuiteXmlFile=src/test/resources/testng.xml
```

---

## 📡 REST API Endpoints

### Base URL
```
http://localhost:8080
```

### Available Endpoints

#### 1. **Status Check Endpoint**
- **Endpoint**: `GET /items`
- **Description**: Check if the API server is running
- **Request**: No body required
- **Response**: Plain text status message

**Example:**
```bash
curl -X GET http://localhost:8080/items
```

**Response:**
```
Displaying Controller Success
```

---

#### 2. **Contact Information Endpoint**
- **Endpoint**: `POST /items/contactInfo`
- **Description**: Submit and echo contact information
- **Content-Type**: `application/json`
- **Request Body**: JSON Contact object

**Request Example:**
```bash
curl -X POST http://localhost:8080/items/contactInfo \
  -H "Content-Type: application/json" \
  -d '{
    "firstName": "John",
    "lastName": "Doe",
    "email": "john.doe@example.com",
    "phoneNumber": "+1-555-0123"
  }'
```

**Response (200 OK):**
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "phoneNumber": "+1-555-0123"
}
```

**Response Status Codes:**
- `200 OK` - Successfully processed
- `400 Bad Request` - Invalid JSON format
- `415 Unsupported Media Type` - Wrong Content-Type header

### API Documentation with Swagger
Once the application is running, access the interactive Swagger UI:
```
http://localhost:8080/swagger-ui/index.html
```

---

## 🧬 Data Model

### Contact Class
Represents contact information with the following fields:

```java
public class Contact {
    private String firstName;      // First name of the contact
    private String lastName;       // Last name of the contact
    private String email;          // Email address
    private String phoneNumber;    // Phone number
}
```

**Example JSON:**
```json
{
  "firstName": "Jane",
  "lastName": "Smith",
  "email": "jane.smith@example.com",
  "phoneNumber": "+1-555-9876"
}
```

---

## 🧪 Test Framework Details

### TestNG Features Used

#### 1. **Data Provider Pattern**
The `SalesForceDataProvider` class provides test data from JSON files:

```java
@Test(dataProvider = "salesForceData", dataProviderClass = SalesForceDataProvider.class)
public void testSalesForceForms(JsonNode jsonObject) {
    // Test implementation
}
```

**Benefits:**
- Parameterized testing with multiple datasets
- Separate test logic from test data
- Easy to add/remove test cases without code changes
- Parallel execution support

#### 2. **Test Data Structure**
Test data is stored as JSON files in:
```
src/test/resources/json/salesforce/
```

Each JSON file represents one test case with contact information:
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "phoneNumber": "+1-555-0123"
}
```

#### 3. **Assertions**
The test uses TestNG assertions to validate responses:

```java
Assert.assertEquals(statusCode, 200);                    // Verify HTTP status
Assert.assertEquals(timeInS, timeInMS / 1000);          // Verify time calculation
```

#### 4. **Response Validation**
Tests validate:
- HTTP Status Code (expecting 200)
- Response Time
- Response Headers
- Response Body Content
- JSON Response Structure

---

## 🔒 Security Configuration

### Security Settings

The application uses Spring Security with the following configuration:

**In SecurityConfig.java:**
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.csrf().disable();                                  // CSRF protection disabled
        http.headers().httpStrictTransportSecurity().disable(); // HSTS disabled
        http.authorizeRequests()
            .requestMatchers(antMatcher("/"))
            .permitAll();                                       // Allow all root requests
        return http.build();
    }
}
```

### Security Notes
⚠️ **Important**: The security configuration disables CSRF and HSTS. This setup is suitable for **testing environments only**.

For **production**, enable:
```java
// Enable CSRF protection
http.csrf().enable();

// Enable HSTS headers
http.headers().httpStrictTransportSecurity().enable();

// Implement proper authorization
http.authorizeRequests()
    .requestMatchers(antMatcher("/api/**")).authenticated()
    .anyRequest().permitAll();
```

---

## 📊 Example Test Execution

### Running a Complete Test Suite

```bash
# Start the server
mvn spring-boot:run &

# Wait for server to start (a few seconds)

# Run all tests
mvn test

# Output example:
# -------------------------------------------------------
# T E S T S
# -------------------------------------------------------
# Running com.restapitest.RestAPITest
# [TestNG]
# testSalesForceForms[0] PASSED
# testSalesForceForms[1] PASSED
# testSalesForceForms[2] PASSED
# Tests run: 3, Failures: 0, Skipped: 0
```

---

## 🔧 Configuration Properties

### Application Configuration
Edit `src/main/resources/application.properties` or `application.yml`:

```properties
# Server Configuration
server.port=8080
server.servlet.context-path=/

# Logging
logging.level.root=INFO
logging.level.com.restapitest=DEBUG

# Actuator
management.endpoints.web.exposure.include=health,metrics

# Swagger
springdoc.api-docs.path=/v3/api-docs
springdoc.swagger-ui.path=/swagger-ui.html
```

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| **Port 8080 already in use** | Change port: `server.port=8081` in properties |
| **Test data not found** | Ensure JSON files exist in `src/test/resources/json/salesforce/` |
| **Connection refused error** | Make sure the server is running on localhost:8080 |
| **Compilation errors with Java version** | Update JAVA_HOME and pom.xml to use Java 17+ |
| **Maven build fails** | Run `mvn clean install -U` to update dependencies |

---

## 📚 REST Assured Testing Library

The project uses **REST Assured** for API testing, which provides a fluent API for testing REST services.

### Key REST Assured Features Used

```java
// Build request specification
RequestSpecBuilder requestSpecBuilder = new RequestSpecBuilder();
requestSpecBuilder.setBaseUri("http://localhost:8080");
requestSpecBuilder.setContentType(ContentType.JSON);
RequestSpecification spec = requestSpecBuilder.build();

// Make a request and validate response
Response response = RestAssured.given()
    .spec(spec)
    .contentType(ContentType.JSON)
    .body(jsonObject)
    .when()
    .post("/items/contactInfo")
    .then()
    .log().all()
    .extract()
    .response();

// Extract response details
int statusCode = response.getStatusCode();
long responseTime = response.time();
String contentType = response.getHeader("content-type");
String body = response.getBody().prettyPrint();
```

---

## 🎯 Development Workflow

### Typical Development Cycle

1. **Start Development Server**
   ```bash
   mvn spring-boot:run
   ```

2. **Make Code Changes**
    - Modify controller, model, or configuration files
    - Hot reload works if using IDE with spring-boot-devtools

3. **Test Changes Manually**
   ```bash
   curl -X POST http://localhost:8080/items/contactInfo \
     -H "Content-Type: application/json" \
     -d '{"firstName":"Test","lastName":"User","email":"test@example.com","phoneNumber":"555-1234"}'
   ```

4. **Run Test Suite**
   ```bash
   mvn test
   ```

5. **Check Swagger Documentation**
   ```
   http://localhost:8080/swagger-ui/index.html
   ```

6. **Build Final Artifact**
   ```bash
   mvn clean install
   ```

---

## 📋 Common Maven Commands

```bash
# Clean and build
mvn clean install

# Run the application
mvn spring-boot:run

# Run tests only
mvn test

# Skip tests during build
mvn clean install -DskipTests

# Run specific test
mvn -Dtest=RestAPITest test

# Show dependency tree
mvn dependency:tree

# Update dependencies to latest versions
mvn clean install -U

# Package as executable JAR
mvn clean package

# Run generated JAR
java -jar target/restapi-test-service-0.0.1-SNAPSHOT.jar
```

---

## 📝 Summary

The **AutomationTesting** project is a complete REST API testing solution that demonstrates:

✅ **REST API Development** - Building and securing REST endpoints  
✅ **Test Automation** - Automated testing with TestNG and REST Assured  
✅ **Data-Driven Testing** - Parameterized tests with JSON data providers  
✅ **Best Practices** - Security, logging, configuration, API documentation  
✅ **Spring Boot Integration** - Modern Java web framework with Spring ecosystem

---

## 📞 Additional Resources

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [TestNG Documentation](https://testng.org/doc/)
- [REST Assured Guide](https://rest-assured.io/)
- [Spring Security Reference](https://spring.io/projects/spring-security)
- [Maven Official Guide](https://maven.apache.org/guides/)

---

**Last Updated**: June 2026  
**Java Version**: 17+  
**Spring Boot Version**: 3.1.4
