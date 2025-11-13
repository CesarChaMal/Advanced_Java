# CLAUDE.md - AI Assistant Guide for Advanced Java Repository

> **Repository Purpose**: O'Reilly Advanced Java video course companion project by Ken Kousen (Spring 2016)
> **Last Updated**: 2025-11-13
> **Build Tool**: Gradle with Groovy support
> **Java Version**: 1.8+ (sourceCompatibility: 1.8)

---

## Table of Contents

1. [Quick Reference](#quick-reference)
2. [Repository Overview](#repository-overview)
3. [Project Structure](#project-structure)
4. [Development Workflows](#development-workflows)
5. [Testing Conventions](#testing-conventions)
6. [Database Configuration](#database-configuration)
7. [Key Design Patterns](#key-design-patterns)
8. [Coding Conventions](#coding-conventions)
9. [Common Tasks](#common-tasks)
10. [Important Constraints](#important-constraints)

---

## Quick Reference

### Essential Commands
```bash
./gradlew build          # Compile, test, generate HTML report at build/reports/tests/
./gradlew run            # Initialize MySQL database with seed data
./gradlew test           # Run all tests
./gradlew test --tests EmployeeTest  # Run specific test class
./gradlew clean          # Clean build artifacts
```

### Key Dependencies
- **Groovy**: 2.5.9
- **Hibernate JPA**: 5.6.10.Final
- **MySQL Connector**: 8.0.30
- **JUnit**: 4.13.2
- **Spock Framework**: 1.3-groovy-2.5
- **JSON API**: javax.json-api 1.1.4

### Database Connection
- **URL**: `jdbc:mysql://localhost:3306/hr`
- **Username**: `jpa`
- **Password**: `java`
- **Required**: MySQL server running on localhost:3306

---

## Repository Overview

This is a **structured learning repository** demonstrating advanced Java concepts including:

- **Lambdas & Streams** - Functional programming with Java 8+
- **Concurrency** - Threading, locks, executors, blocking queues
- **Generics** - Type-safe collections, bounded types, wildcards
- **Database Access** - Both JDBC and JPA/Hibernate patterns
- **OOP Fundamentals** - Inheritance, inner classes, interfaces
- **Networking** - Sockets, HTTP, URL handling, API integration
- **New I/O (NIO)** - Modern file operations with Path/Files API

### Architecture Philosophy
- **One topic per package** - Clear separation of concerns
- **Demo-centric** - Most classes have `main()` methods for exploration
- **Paired implementations** - Shows multiple approaches (JDBC vs JPA, synchronized vs atomic vs locks)
- **Test-driven** - Tests serve as documentation of expected behavior
- **Interface-first** - Abstract patterns behind interfaces for flexibility

---

## Project Structure

### Source Layout

```
src/
├── main/
│   ├── java/                    # Primary Java source (81 files)
│   │   ├── concurrency/         # 17 files - Threading, locks, executors
│   │   │   ├── locks/           # SyncCounter, AtomicCounter, LockedCounter, LocksDemo
│   │   │   ├── callables/       # FileLinesCounter, callable pattern
│   │   │   ├── latch/           # CountDownLatch worker demo
│   │   │   └── blockingqueue/   # Producer-Consumer with LinkedBlockingQueue
│   │   ├── lambdasstreams/      # 12 files - Streams API, lambdas, method references
│   │   ├── generics/            # 11 files - Generic types, bounded parameters
│   │   │   ├── repairables/     # Generic repair shop pattern
│   │   │   └── wildcards/       # HR class with wildcard usage
│   │   ├── oop/                 # 7 files - Employee hierarchy, equals/hashCode
│   │   ├── database/            # 6 files - Data access layer
│   │   │   ├── jpa/             # JpaPersonDAO, Person entity
│   │   │   └── jdbc/            # JdbcPersonDAO (raw SQL)
│   │   ├── innerclasses/        # 6 files - Member, static, anonymous inner classes
│   │   ├── interfaces/          # 6 files - Interface design, default methods
│   │   │   └── defaults/        # Java 8 default method demos
│   │   ├── networking/          # 4 files - Sockets, URLs, Geocoding API
│   │   │   └── sockets/         # EchoServer, EchoClient
│   │   ├── newio/               # 2 files - Path, Files API
│   │   └── exceptions/          # 2 files - Custom exception handling
│   ├── groovy/
│   │   └── database/
│   │       └── initialize_database.groovy  # Database setup script (main class)
│   └── resources/
│       └── META-INF/
│           └── persistence.xml  # JPA/Hibernate configuration
└── test/
    ├── java/                    # JUnit 4 tests (9 files)
    │   ├── junit/               # Basic assertion tests
    │   ├── database/            # DAO integration tests
    │   ├── lambdasstreams/      # Stream operations tests
    │   ├── networking/          # Geocoder API tests
    │   └── oop/                 # Employee class tests
    └── groovy/
        └── DBSpec.groovy        # Spock database connectivity test
```

### Package Responsibilities

| Package | Purpose | Key Classes |
|---------|---------|-------------|
| `concurrency` | Thread management, synchronization patterns | UseExecutors, LocksDemo, ProducerConsumer |
| `lambdasstreams` | Functional programming with Java 8+ | LambdasDemo, StreamsDemo, SortingDemo |
| `generics` | Type-safe collections and algorithms | Pair, Tuple, RepairShop, HR |
| `oop` | Object-oriented fundamentals | Employee (abstract), Salaried, Hourly |
| `database.jpa` | JPA/Hibernate data access | JpaPersonDAO, Person (entity) |
| `database.jdbc` | Raw JDBC data access | JdbcPersonDAO |
| `innerclasses` | Inner class patterns | StringSorter, OuterInner |
| `interfaces.defaults` | Java 8 default methods | CompanyDefaultMethods, Company |
| `networking` | Network communication | URLSplitter, Geocoder, EchoServer |
| `newio` | Modern file I/O | PathOperations, MorePathOperations |
| `exceptions` | Exception handling patterns | Custom exceptions, arithmetic error handling |

---

## Development Workflows

### Standard Development Cycle

1. **Before Starting**:
   - Ensure MySQL is running on localhost:3306
   - Initialize database: `./gradlew run`
   - Verify connectivity: `./gradlew test --tests DBSpec`

2. **Making Changes**:
   - Maintain package structure (mirror test structure in src/test)
   - Add tests for new functionality
   - Follow existing naming conventions
   - Include JavaDoc for public APIs

3. **Testing**:
   - Run full test suite: `./gradlew build`
   - Run specific tests: `./gradlew test --tests <TestClassName>`
   - Check HTML report: `build/reports/tests/index.html`

4. **Committing**:
   - Ensure all tests pass
   - Follow existing commit message style (see git log)
   - Include test updates with code changes

### Adding New Demo Classes

Most packages contain demo classes with `main()` methods for learning. When adding new demos:

```java
package <topic>;

public class NewConceptDemo {
    public static void main(String[] args) {
        // Demonstrate concept clearly
        System.out.println("Output should be educational");

        // Include comments explaining what's happening
        // Show both correct and incorrect usage if relevant
    }
}
```

### Adding New Tests

Follow the existing pattern:

```java
// For JUnit 4 tests
package <topic>;

import org.junit.Test;
import static org.junit.Assert.*;

public class NewFeatureTest {
    @Test
    public void testFeatureBehavior() {
        // Arrange
        // Act
        // Assert with descriptive message
        assertEquals("descriptive failure message", expected, actual);
    }
}
```

---

## Testing Conventions

### Test Framework Mix

**JUnit 4** (Primary - 9 test files):
```java
import org.junit.Test;
import org.junit.Ignore;
import static org.junit.Assert.*;
import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.*;

@Test
public void testMethod() {
    assertEquals("failure message", expected, actual);
    assertThat(actual, equalTo(expected));
}
```

**Spock Framework** (Groovy BDD - 1 test file):
```groovy
import spock.lang.Specification

class DBSpec extends Specification {
    def 'verify database exists'() {
        expect:
        // BDD-style assertions
    }
}
```

### Test Patterns Used

1. **Package Mirroring**: Test packages mirror main packages
   - `src/main/java/database/jpa/JpaPersonDAO.java`
   - `src/test/java/database/jpa/JpaPersonDAOTest.java`

2. **Naming Conventions**:
   - Test classes: `*Test` suffix (e.g., `EmployeeTest`)
   - Test methods: Descriptive names (`testJoinStream()`, `getAndSetName()`)
   - Spock specs: Natural language in single quotes

3. **Assertion Patterns**:
   - Always include descriptive failure messages
   - Use Hamcrest matchers for complex assertions
   - Use `closeTo()` for floating-point comparisons
   - Null checks before assertions: `assertNotNull(result)`

4. **Database Testing**:
   - Database initialized with 6 seed records (Star Trek characters)
   - Integration tests assume fresh database state
   - Both JDBC and JPA DAOs tested against same interface

5. **Test Data**:
   - Seed data in `initialize_database.groovy`:
     - Picard, Riker, Troi, Laforge, Data, Crusher
   - Tests verify count (6 records) and specific names

### Running Tests

```bash
# All tests
./gradlew test

# Specific test class
./gradlew test --tests EmployeeTest

# Specific test method
./gradlew test --tests EmployeeTest.getAndSetName

# With HTML report generation
./gradlew build
# Report: build/reports/tests/index.html

# Clean and test
./gradlew clean test
```

---

## Database Configuration

### MySQL Setup Required

The project requires a MySQL database named `hr` running locally:

```
Host: localhost
Port: 3306
Database: hr
Username: jpa
Password: java
```

### Persistence Configuration

**File**: `src/main/resources/META-INF/persistence.xml`

```xml
<persistence-unit name="hr" transaction-type="RESOURCE_LOCAL">
    <provider>org.hibernate.jpa.HibernatePersistenceProvider</provider>
    <class>database.jpa.Person</class>

    <properties>
        <property name="javax.persistence.jdbc.url"
                  value="jdbc:mysql://localhost:3306/hr"/>
        <property name="javax.persistence.jdbc.user" value="jpa"/>
        <property name="javax.persistence.jdbc.password" value="java"/>
        <property name="javax.persistence.jdbc.driver"
                  value="com.mysql.jdbc.Driver"/>

        <!-- Hibernate properties -->
        <property name="hibernate.show_sql" value="true"/>
        <property name="hibernate.format_sql" value="true"/>
        <property name="hibernate.dialect"
                  value="org.hibernate.dialect.MySQL5InnoDBDialect"/>
        <property name="hibernate.hbm2ddl.auto" value="validate"/>
    </properties>
</persistence-unit>
```

**Key Settings**:
- `show_sql: true` - Logs SQL statements (helpful for debugging)
- `format_sql: true` - Makes SQL readable in logs
- `hbm2ddl.auto: validate` - Requires schema to exist (doesn't auto-create)

### Database Initialization

**File**: `src/main/groovy/database/initialize_database.groovy`

**Purpose**: Drops and recreates PEOPLE table with seed data

**Schema**:
```sql
CREATE TABLE PEOPLE (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(200)
)
```

**Seed Data** (6 records):
- Jean-Luc Picard
- William Riker
- Deanna Troi
- Geordi Laforge
- Data Soong
- Beverly Crusher

**Run**: `./gradlew run`

### DAO Pattern

**Shared Interface** (`database.PersonDAO`):
```java
public interface PersonDAO {
    Person findById(int id);
    List<Person> findAll();
    void save(Person person);
    void delete(int id);
}
```

**Two Implementations**:

1. **JDBC Implementation** (`database.jdbc.JdbcPersonDAO`)
   - Direct SQL: `SELECT * FROM PEOPLE`, `INSERT INTO PEOPLE`, etc.
   - Manual Connection, Statement, ResultSet handling
   - Try-catch for SQLException

2. **JPA Implementation** (`database.jpa.JpaPersonDAO`)
   - EntityManager API
   - JPQL: `SELECT p FROM Person p`
   - Transaction management: `em.getTransaction().begin()/commit()`
   - Entity-based (no manual SQL)

**When to Use Which**:
- Use **JDBC** when you need fine-grained SQL control or working with legacy databases
- Use **JPA** for object-oriented approach with entity relationships and caching

---

## Key Design Patterns

### 1. Data Access Object (DAO)

**Location**: `database.*` packages

**Pattern**:
```java
// Interface
public interface PersonDAO {
    List<Person> findAll();
    Person findById(int id);
    void save(Person person);
    void delete(int id);
}

// JDBC Implementation
public class JdbcPersonDAO implements PersonDAO {
    // Direct SQL
}

// JPA Implementation
public class JpaPersonDAO implements PersonDAO {
    // EntityManager
}
```

**Benefits**:
- Abstracts database access
- Allows swapping implementations
- Testable (can mock interface)

### 2. Producer-Consumer

**Location**: `concurrency.blockingqueue.*`

**Pattern**:
```java
BlockingQueue<Item> queue = new LinkedBlockingQueue<>();

// Producer
new Thread(() -> queue.put(item)).start();

// Consumer
new Thread(() -> queue.take()).start();
```

**Use Case**: Thread-safe communication between threads

### 3. Template Method

**Location**: `oop.Employee` (abstract class)

**Pattern**:
```java
abstract class Employee {
    // Template method
    public final double calculateBonus() {
        return getSalary() * getBonusMultiplier();
    }

    // Abstract methods for subclasses
    abstract double getSalary();
    abstract double getBonusMultiplier();
}
```

### 4. Strategy

**Location**: `lambdasstreams.*` (functional interfaces)

**Pattern**:
```java
// Strategy as lambda
Consumer<String> strategy = s -> System.out.println(s);

// Pluggable behavior
list.forEach(strategy);
```

### 5. Comparison Pattern

**Location**: `concurrency.locks.LocksDemo`

**Purpose**: Compare three synchronization approaches
- `SyncCounter` - synchronized methods
- `AtomicCounter` - AtomicInteger
- `LockedCounter` - ReentrantLock

**Use**: Demonstrates trade-offs between different implementations

---

## Coding Conventions

### Naming Conventions

| Type | Convention | Examples |
|------|------------|----------|
| Classes | PascalCase | `Employee`, `PersonDAO`, `LockedCounter` |
| Methods | camelCase | `findAll()`, `getAnInt()`, `calculateBonus()` |
| Constants | UPPER_SNAKE_CASE | `DEFAULT_NAME`, `MAX_CONNECTIONS` |
| Variables | camelCase | `personList`, `employeeName` |
| Test Classes | `*Test` suffix | `EmployeeTest`, `StreamsDemoTest` |
| Test Methods | Descriptive | `testJoinStream()`, `getAndSetName()` |
| Interfaces | Name or `I` prefix | `PersonDAO`, `Runnable` |
| Implementations | Descriptive | `JdbcPersonDAO`, `JpaPersonDAO` |

### Class Design Principles

1. **Always Implement Object Methods**:
```java
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    // Field comparison
}

@Override
public int hashCode() {
    int result = field1.hashCode();
    result = 31 * result + field2.hashCode();  // Always use 31 multiplier
    return result;
}

@Override
public String toString() {
    return "ClassName{field1=" + field1 + ", field2=" + field2 + "}";
}
```

2. **JPA Entity Requirements**:
```java
@Entity
@Table(name = "PEOPLE")
public class Person {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    // No-arg constructor required for JPA
    public Person() {}

    // Getters and setters
}
```

3. **Resource Management**:
```java
// Always use try-with-resources for auto-closeable
try (InputStream is = url.openStream();
     BufferedReader reader = new BufferedReader(new InputStreamReader(is))) {
    // Use resources
} // Automatically closed

// ExecutorService cleanup
ExecutorService service = Executors.newCachedThreadPool();
try {
    // Use service
} finally {
    service.shutdown();
}
```

### Java 8+ Feature Usage

**Lambdas**:
```java
// Prefer concise lambdas
list.forEach(s -> System.out.println(s));

// Use method references when possible
list.forEach(System.out::println);
```

**Streams**:
```java
// Fluent API chaining
list.stream()
    .filter(s -> s.startsWith("A"))
    .map(String::toUpperCase)
    .collect(Collectors.toList());
```

**Functional Interfaces**:
```java
Consumer<String> consumer = s -> System.out.println(s);
Function<String, Integer> function = String::length;
Predicate<String> predicate = s -> s.isEmpty();
```

**Default Methods in Interfaces**:
```java
interface MyInterface {
    // Default implementation
    default void defaultMethod() {
        System.out.println("Default behavior");
    }
}
```

**Try-with-Resources** (Java 7+):
```java
try (Connection conn = DriverManager.getConnection(url);
     Statement stmt = conn.createStatement()) {
    // Use connection
}
```

### Concurrency Best Practices

1. **Prefer ExecutorService over raw Threads**:
```java
// Good
ExecutorService service = Executors.newCachedThreadPool();
service.submit(runnable);
service.shutdown();

// Avoid (unless necessary)
new Thread(runnable).start();
```

2. **Choose Appropriate Synchronization**:
- **synchronized** - Simple, method-level locking
- **AtomicInteger** - Single variable, high contention
- **ReentrantLock** - Complex locking logic, fairness needed
- **BlockingQueue** - Producer-consumer communication

3. **Always Clean Up**:
```java
service.shutdown();
service.awaitTermination(1, TimeUnit.MINUTES);
```

### Error Handling

```java
// Catch specific exceptions
try {
    // Operation
} catch (SQLException e) {
    // Log informative message
    System.err.println("Database error: " + e.getMessage());
    e.printStackTrace();
    // Consider rethrowing as runtime exception
    throw new RuntimeException("Failed to access database", e);
}

// Null checks in tests
Person person = dao.findById(1);
assertNotNull("Person should exist", person);
assertEquals("Picard", person.getName());
```

### Code Organization

1. **One public class per file** (standard Java)
2. **Package-private helper classes** allowed in same file
3. **Static imports** for test assertions:
```java
import static org.junit.Assert.*;
import static org.hamcrest.Matchers.*;
```

4. **Main methods for demos**:
```java
public static void main(String[] args) {
    // Educational demonstration code
}
```

---

## Common Tasks

### Adding a New Topic/Package

1. Create package under `src/main/java/<newtopic>`
2. Add demo class with `main()` method
3. Create corresponding test package: `src/test/java/<newtopic>`
4. Add test class with JUnit tests
5. Update this CLAUDE.md file with package description

### Implementing a New Database Entity

1. **Create Entity Class**:
```java
package database.jpa;

@Entity
@Table(name = "TABLE_NAME")
public class NewEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    // Fields, no-arg constructor, getters/setters, equals/hashCode
}
```

2. **Add to persistence.xml**:
```xml
<class>database.jpa.NewEntity</class>
```

3. **Create DAO Interface**:
```java
public interface NewEntityDAO {
    NewEntity findById(int id);
    List<NewEntity> findAll();
    void save(NewEntity entity);
    void delete(int id);
}
```

4. **Implement JDBC and JPA versions**

5. **Add Tests**

6. **Update initialize_database.groovy** if seed data needed

### Running Individual Demo Classes

Most packages have demo classes. To run them:

```bash
# Build first
./gradlew build

# Run class with main method
java -cp build/classes/java/main concurrency.UseExecutors
java -cp build/classes/java/main lambdasstreams.LambdasDemo
java -cp build/classes/java/main generics.wildcards.HR

# Or configure in IDE (IntelliJ/Eclipse) to run main methods directly
```

### Debugging Database Issues

1. **Check MySQL is running**:
```bash
mysql -u jpa -pjava -h localhost -P 3306
```

2. **Reinitialize database**:
```bash
./gradlew run
```

3. **Verify connectivity**:
```bash
./gradlew test --tests DBSpec
```

4. **Check Hibernate logs** (show_sql=true in persistence.xml):
- SQL statements printed to console
- Verify correct queries being generated

5. **Common Issues**:
- MySQL not running: Start MySQL service
- Wrong credentials: Check persistence.xml (jpa/java)
- Schema not found: Run `./gradlew run` to create
- Port conflict: Ensure 3306 is free

### Adding External Dependencies

1. **Update build.gradle**:
```gradle
dependencies {
    compile 'group:artifact:version'
    // or
    testCompile 'group:artifact:version'
}
```

2. **Refresh Gradle**:
```bash
./gradlew --refresh-dependencies
```

3. **Note**: Dependencies auto-monitored by Dependabot (configured in `.github/dependabot.yml`)

---

## Important Constraints

### What to Preserve

1. **Java 8 Compatibility**:
   - `sourceCompatibility = 1.8`
   - No Java 9+ features (modules, var, records, etc.)
   - Stick to Java 8 APIs

2. **Database Configuration**:
   - Do not change persistence.xml connection details
   - Maintain MySQL compatibility
   - Keep seed data for test consistency

3. **Test Framework Mix**:
   - Keep both JUnit and Spock tests
   - Demonstrates framework diversity
   - Do not migrate all tests to one framework

4. **Package Structure**:
   - Maintain topic-based organization
   - One concern per package
   - Mirror structure in tests

5. **Educational Purpose**:
   - Keep main() methods for demos
   - Preserve clear, simple examples
   - Avoid over-engineering

### What Can Be Modified

1. **Dependency Versions**:
   - Can upgrade within compatibility constraints
   - Dependabot handles security updates
   - Test thoroughly after upgrades

2. **New Examples**:
   - Add new demo classes
   - Create new test cases
   - Extend existing patterns

3. **Code Quality**:
   - Refactor for clarity
   - Add JavaDoc
   - Improve error handling
   - Add more tests

4. **Build Configuration**:
   - Gradle task customization
   - Add plugins (if beneficial)
   - Test configuration

### Known Issues/Limitations

1. **Deprecated APIs**:
   - `com.mysql.jdbc.Driver` deprecated (use `com.mysql.cj.jdbc.Driver`)
   - Consider updating in persistence.xml

2. **Java 8 Only**:
   - Modern Java features not available
   - Intentional for course compatibility

3. **Local Database Required**:
   - Tests need MySQL running
   - Not portable without database
   - Consider H2 in-memory for testing (future enhancement)

4. **JCenter Repository**:
   - JCenter is deprecated
   - Consider migrating to Maven Central

5. **No Containerization**:
   - Could benefit from Docker for database
   - Would improve portability

---

## For AI Assistants: Best Practices

### When Making Changes

1. **Understand Context**:
   - This is a learning repository
   - Prioritize clarity over cleverness
   - Examples should be educational

2. **Maintain Consistency**:
   - Follow existing patterns
   - Match code style in package
   - Use same test framework as sibling tests

3. **Test Thoroughly**:
   - Run `./gradlew build` before committing
   - Add tests for new functionality
   - Verify database connectivity if touching DB code

4. **Document Changes**:
   - Add JavaDoc for public APIs
   - Include inline comments for complex logic
   - Update this CLAUDE.md if structure changes

### When Explaining Code

1. **Reference Specific Files**:
   - Use `package.ClassName:lineNumber` format
   - Example: "The DAO pattern is in `database.PersonDAO:5-10`"

2. **Connect to Learning Objectives**:
   - Explain which Java feature is being demonstrated
   - Reference relevant Java 8 features
   - Point to related examples in other packages

3. **Provide Context**:
   - Explain why pattern was chosen
   - Compare alternative approaches
   - Link to paired implementations (JDBC vs JPA)

### When Debugging

1. **Check Test Reports**:
   - `build/reports/tests/index.html` has detailed failure info
   - Stack traces show exact failure point

2. **Verify Database State**:
   - Run `./gradlew run` to reset
   - Check MySQL is accessible
   - Verify seed data loaded (6 records)

3. **Review Hibernate Logs**:
   - SQL statements logged to console
   - Verify queries match expectations

4. **Common Gotchas**:
   - EntityManager not closed → memory leak
   - ExecutorService not shutdown → hanging JVM
   - Missing @Entity annotation → JPA errors
   - Wrong persistence unit name → configuration not found

---

## Version History

| Date | Update | Description |
|------|--------|-------------|
| 2025-11-13 | Initial creation | Comprehensive documentation for AI assistants |

---

## Additional Resources

### Project Documentation
- **README.adoc**: Project overview and setup instructions
- **build.gradle**: Build configuration and dependencies
- **persistence.xml**: JPA/Hibernate configuration

### External References
- **Java 8 Documentation**: https://docs.oracle.com/javase/8/docs/
- **Hibernate 5.6**: https://docs.jboss.org/hibernate/orm/5.6/
- **Spock Framework**: https://spockframework.org/
- **JUnit 4**: https://junit.org/junit4/

### Course Information
- **Author**: Ken Kousen
- **Publisher**: O'Reilly Media
- **Topic**: Advanced Java Programming
- **Year**: Spring 2016

---

**End of CLAUDE.md**

*This file should be updated when significant structural changes are made to the repository.*
