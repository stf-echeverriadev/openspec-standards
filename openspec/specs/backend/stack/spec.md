# Backend Stack Specification

## Purpose
Defines the baseline technology stack shared by every backend project. All
backend applications are Spring Boot services written in Java and built with
Maven, so that knowledge, tooling, and conventions transfer between projects the
same way the frontend stack does.

## Requirements

### Requirement: Java 21 as the language and runtime
Backend applications MUST be written in Java targeting the Java 21 LTS release.
Projects MUST NOT target an older Java version or introduce another JVM language
(e.g. Kotlin, Scala) for new code without a change proposal in this store.

#### Scenario: New service
- GIVEN a developer starts a new backend service
- WHEN the project is configured
- THEN it compiles and runs against Java 21
- AND it does not target an older Java version

### Requirement: Spring Boot as the framework
Backend applications MUST be built with Spring Boot. Plain servlet stacks or
alternative frameworks (e.g. Quarkus, Micronaut) MUST NOT be introduced without
a change proposal in this store.

#### Scenario: Bootstrapping a service
- GIVEN a new backend service
- WHEN it is bootstrapped
- THEN it is a Spring Boot application
- AND application configuration is provided through Spring Boot configuration
  files

### Requirement: Maven as the build tool
Projects MUST use Maven for dependency management and builds, and MUST commit the
Maven Wrapper (`mvnw`) so builds are reproducible without a preinstalled Maven.
Gradle and custom build scripts MUST NOT be used without a change proposal.

#### Scenario: Reproducible build
- GIVEN a fresh checkout of a backend project
- WHEN the documented build command (e.g. `./mvnw verify`) runs
- THEN Maven resolves the declared dependencies and builds the service
- AND no globally installed Maven is required

### Requirement: Dependency versions managed by Spring Boot
Dependency versions MUST be governed by the Spring Boot dependency management
(parent POM or imported BOM) rather than pinned ad hoc per dependency, so that
versions stay mutually compatible.

#### Scenario: Adding a Spring-managed dependency
- GIVEN a developer adds a Spring-managed dependency
- WHEN they declare it in the POM
- THEN its version comes from the Spring Boot dependency management
- AND a hard-coded version is only added when the dependency is not managed by
  Spring Boot
