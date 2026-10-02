# Oberon OSS BOMs

Bill of Materials (BOM) and parent POM for **Oberon OSS** Java projects.

This project standardizes dependency versions, plugin configurations, build settings, and testing frameworks across all
Oberon OSS repositories.

---

## Prerequisites

- **Java**: 25 or higher
- **Maven**: 3.9 or higher

---

## Usage

This project provides two distinct artifacts:

### 1. As a Parent POM (Recommended for Oberon OSS projects)

Inheriting from `parent` provides complete build orchestration, including pre-configured compiler settings (Java 25,
Lombok annotation processing), standard testing frameworks (JUnit 5, Mockito, AssertJ), code coverage (JaCoCo),
repository distribution management, and automatically imports the `bom`.

Add the following to your `pom.xml`:

```xml
<parent>
    <groupId>eu.oberon-oss</groupId>
    <artifactId>parent</artifactId>
    <version>3.25.13</version>
</parent>
```

#### What you get as a child project:

- **Default Dependencies**: Automatically includes `org.jetbrains:annotations` (compile) and standard test dependencies
  (`junit-jupiter`, `mockito-core`, `mockito-junit-jupiter`, `assertj-core`).
- **Dependency Management**: Automatically imports `eu.oberon-oss:bom`.
- **Compiler Configuration**: Configured for Java 25 source/target with Lombok annotation processor path enabled.
- **Surefire / Test Runner**: Pre-configured with Mockito Java agent and JVM flags (`-Xshare:off`,
  `--sun-misc-unsafe-memory-access=allow`).
- **Default Profiles**: JaCoCo agent and reporting enabled by default (`jacoco`), as well as distribution management for
  Oberon OSS Nexus (`activate-oberon-nexus`).

---

### 2. As an Imported BOM (Dependency Management)

If you only want to align dependency versions without inheriting build plugins and parent configuration, import `bom`
into your `<dependencyManagement>` section:

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>eu.oberon-oss</groupId>
            <artifactId>bom</artifactId>
            <version>3.25.13</version>
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

## Project Structure & Artifacts

The repository is organized as a multi-module Maven project providing two distinct artifacts:

| Artifact | Maven Coordinates | Purpose & Contents |
|:---------|:------------------|:-------------------|
| **BOM** | `eu.oberon-oss:bom` | Standalone Bill of Materials providing library version management via `<dependencyManagement>`. |
| **Parent POM** | `eu.oberon-oss:parent` | Complete project parent providing build lifecycle plugins, compiler configuration, test agents, default dependencies, and automatic BOM import. |

---

## Managed Dependencies

The `eu.oberon-oss:bom` artifact manages versions for commonly used libraries across Oberon OSS projects:

| Category            | Artifact                                    | Description                                            |
|:--------------------|:--------------------------------------------|:-------------------------------------------------------|
| **Annotations**     | `jakarta.annotation:jakarta.annotation-api` | Common Jakarta annotations                             |
| **Annotations**     | `org.jetbrains:annotations`                 | JetBrains nullability and inspection annotations       |
| **Code Generation** | `org.projectlombok:lombok`                  | Boilerplate reduction annotations & compiler processor |
| **CLI**             | `info.picocli:picocli`                      | Command line application framework                     |
| **Logging**         | `org.slf4j:slf4j-api`                       | Standard logging API                                   |
| **Testing**         | `org.junit.jupiter:junit-jupiter`           | JUnit 5 testing framework (API, Engine, Aggregator)    |
| **Testing**         | `org.junit-pioneer:junit-pioneer`           | JUnit 5 extension pack                                 |
| **Testing**         | `org.mockito:mockito-core`                  | Mocking framework                                      |
| **Testing**         | `org.mockito:mockito-junit-jupiter`         | Mockito JUnit 5 extension                              |
| **Testing**         | `org.assertj:assertj-core`                  | Fluent assertions for Java                             |
| **Testing**         | `io.github.hakky54:logcaptor`               | Log output capturing utility for unit testing          |
| **Build Tools**     | `eu.oberon-oss.tools:git-commit-in-log`     | Git commit logging utility                             |

---

## Managed Plugins & Build Configuration

The `eu.oberon-oss:parent` POM (backed by root `<pluginManagement>`) configures essential build plugins:

- **Compiler**: `maven-compiler-plugin` configured for Java 25 with Lombok annotation processor path enabled.
- **Testing**: `maven-surefire-plugin` with Mockito agent attachments and JVM execution flags (`-Xshare:off`, `--sun-misc-unsafe-memory-access=allow`).
- **Quality & Coverage**: `jacoco-maven-plugin` and `sonar-maven-plugin`.
- **Enforcer**: `maven-enforcer-plugin` enforcing minimum Java 25, Maven 3.9, and banning duplicate POM dependency versions.
- **Git Metadata**: `git-commit-id-maven-plugin` for build-time Git commit extraction.
- **Packaging & Publishing**: `central-publishing-maven-plugin`, `maven-gpg-plugin`, `maven-source-plugin`, `maven-javadoc-plugin`, `maven-assembly-plugin`, and `versions-maven-plugin`.

---

## Maven Profiles

| Profile ID                    | Provided In | Description                                                         | Default Status       |
|:------------------------------|:------------|:--------------------------------------------------------------------|:---------------------|
| `activate-oberon-nexus`       | Root / Parent | Configures distribution management for Oberon OSS Nexus repository. | Active by default    |
| `jacoco`                      | Parent      | Configures JaCoCo agent and test reporting.                         | Active by default    |
| `include-git-commit-data`     | Parent      | Generates `git.properties` and includes `git-commit-in-log`.        | Inactive (on demand) |
| `activate-maven-central`      | Root        | Switches distribution management to Sonatype / Maven Central.       | Inactive (on demand) |
| `central-release`             | Root        | Enables Central publishing plugin and GPG artifact signing.         | Inactive (on demand) |
| `generate-javadoc-and-source` | Root        | Attaches Javadoc and source JARs to the build package phase.        | Inactive (on demand) |
| `coverage`                    | Root        | Generates JaCoCo XML reports for CI/CD analysis (e.g. SonarCloud).  | Inactive (on demand) |

---

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).
