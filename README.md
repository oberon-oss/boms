# Oberon OSS BOMs

Bill of Materials (BOM) and parent POM for **Oberon OSS** Java projects.

This project standardizes dependency versions, plugin configurations, build settings, and testing frameworks across all Oberon OSS repositories.

---

## Prerequisites

- **Java**: 25 or higher
- **Maven**: 3.9 or higher

---

## Usage

You can use `boms` in two primary ways:

### 1. As a Parent POM (Recommended for Oberon OSS projects)

Inheriting from `boms` as a parent POM provides complete build orchestration, including pre-configured compiler settings (Java 25, Lombok annotation processing), standard testing frameworks (JUnit 5, Mockito, AssertJ), code coverage (JaCoCo), and repository distribution management.

Add the following to your `pom.xml`:

```xml
<parent>
    <groupId>eu.oberon-oss</groupId>
    <artifactId>boms</artifactId>
    <version>3.25.10</version>
</parent>
```

#### What you get as a child project:
- **Default Dependencies**: Automatically includes `org.jetbrains:annotations` (compile) and standard test dependencies (`junit-jupiter`, `mockito-core`, `mockito-junit-jupiter`, `assertj-core`).
- **Compiler Configuration**: Configured for Java 25 source/target with Lombok annotation processor path enabled.
- **Surefire / Test Runner**: Pre-configured with Mockito Java agent and JVM flags (`-Xshare:off`, `--sun-misc-unsafe-memory-access=allow`).
- **Default Profiles**: JaCoCo agent and reporting enabled by default (`jacoco`), as well as distribution management for Oberon OSS Nexus (`activate-oberon-nexus`).

---

### 2. As an Imported BOM (Dependency Management)

If you only want to align dependency versions without inheriting build plugins and parent configuration, import the BOM into your `<dependencyManagement>` section:

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>eu.oberon-oss</groupId>
            <artifactId>boms</artifactId>
            <version>3.25.10</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

Then declare the dependencies you need without specifying versions:

```xml
<dependencies>
    <!-- Logging -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
    </dependency>

    <!-- CLI Tools -->
    <dependency>
        <groupId>info.picocli</groupId>
        <artifactId>picocli</artifactId>
    </dependency>

    <!-- Testing -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.assertj</groupId>
        <artifactId>assertj-core</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

---

## Managed Dependencies

The BOM manages versions for commonly used libraries across Oberon OSS projects:

| Category | Artifact | Description |
| :--- | :--- | :--- |
| **Annotations** | `jakarta.annotation:jakarta.annotation-api` | Common Jakarta annotations |
| **Annotations** | `org.jetbrains:annotations` | JetBrains nullability and inspection annotations |
| **Code Generation** | `org.projectlombok:lombok` | Boilerplate reduction annotations & compiler processor |
| **CLI** | `info.picocli:picocli` | Command line application framework |
| **Logging** | `org.slf4j:slf4j-api` | Standard logging API |
| **Testing** | `org.junit.jupiter:junit-jupiter` | JUnit 5 testing framework (API, Engine, Aggregator) |
| **Testing** | `org.junit-pioneer:junit-pioneer` | JUnit 5 extension pack |
| **Testing** | `org.mockito:mockito-core` | Mocking framework |
| **Testing** | `org.mockito:mockito-junit-jupiter` | Mockito JUnit 5 extension |
| **Testing** | `org.assertj:assertj-core` | Fluent assertions for Java |
| **Testing** | `io.github.hakky54:logcaptor` | Log output capturing utility for unit testing |
| **Build Tools** | `eu.oberon-oss.tools:git-commit-in-log` | Git commit logging utility |

---

## Managed Plugins & Build Tools

The BOM manages and configures essential Maven plugins:

- **Compiler**: `maven-compiler-plugin` configured with Lombok annotation processor.
- **Testing**: `maven-surefire-plugin` with Mockito agent attachments.
- **Quality & Coverage**: `jacoco-maven-plugin` and `sonar-maven-plugin`.
- **Enforcer**: `maven-enforcer-plugin` enforcing minimum Java 25, Maven 3.9, and banning duplicate POM dependency versions.
- **Git Metadata**: `git-commit-id-maven-plugin` for build-time Git commit extraction.
- **Packaging & Publishing**: `central-publishing-maven-plugin`, `maven-gpg-plugin`, `maven-source-plugin`, `maven-javadoc-plugin`, `maven-assembly-plugin`, and `versions-maven-plugin`.

---

## Maven Profiles

| Profile ID | Description | Default Status |
| :--- | :--- | :--- |
| `activate-oberon-nexus` | Configures distribution management for Oberon OSS Nexus repository. | Active by default |
| `jacoco` | Configures JaCoCo agent and test reporting. | Active by default |
| `include-git-commit-data` | Generates `git.properties` and includes `git-commit-in-log`. | Inactive (on demand) |
| `activate-maven-central` | Switches distribution management to Sonatype / Maven Central. | Inactive (on demand) |
| `central-release` | Enables Central publishing plugin and GPG artifact signing. | Inactive (on demand) |
| `generate-javadoc-and-source` | Attaches Javadoc and source JARs to the build package phase. | Inactive (on demand) |
| `coverage` | Generates JaCoCo XML reports for CI/CD analysis (e.g. SonarCloud). | Inactive (on demand) |

---

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).
