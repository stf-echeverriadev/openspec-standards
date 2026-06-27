# Backend Data Specification

## Purpose
Defines how backend projects store and access relational data. All backend
services use SQL Server as the database, access it through Spring Data JPA, and
evolve its schema exclusively through Flyway migrations, so persistence works the
same way across projects.

## Requirements

### Requirement: SQL Server as the database
Backend projects MUST use Microsoft SQL Server as their relational database.
Another relational engine MUST NOT be introduced for application data without a
change proposal in this store.

#### Scenario: Persisting application data
- GIVEN a service that needs to store relational data
- WHEN persistence is configured
- THEN it targets a SQL Server database
- AND no alternative engine is used as the primary store without a change
  proposal

### Requirement: Spring Data JPA for data access
Data access MUST go through Spring Data JPA (Hibernate) repositories rather than
hand-written JDBC scattered across the codebase. Entities and repositories MUST
live in a dedicated persistence layer, and other layers MUST NOT issue raw SQL
directly.

#### Scenario: Reading an entity
- GIVEN a service needs to read a persisted entity
- WHEN it accesses the data
- THEN it uses a Spring Data JPA repository
- AND it does not embed raw JDBC in controllers or business services

### Requirement: Schema changes through Flyway migrations
The database schema MUST be managed exclusively through Flyway migration scripts
committed to the repository. Hibernate auto-DDL (`ddl-auto`) MUST NOT be used to
create or alter schema in any shared or production environment.

#### Scenario: Adding a column
- GIVEN a developer needs a new column
- WHEN they change the schema
- THEN they add a new versioned Flyway migration script
- AND they do not rely on Hibernate to alter the schema automatically

#### Scenario: Applying migrations
- GIVEN a service starts against a database
- WHEN the application boots
- THEN Flyway applies any pending migrations in version order

### Requirement: Database configuration from the environment
Connection details (URL, credentials) MUST be read from externalized
configuration (environment variables or the runtime config source) and MUST NOT
be hard-coded or committed in source.

#### Scenario: Switching environments
- GIVEN the service targets staging instead of production
- WHEN it is deployed to staging
- THEN the database connection comes from environment configuration, not edited
  source code
- AND no credentials are committed to the repository

### Requirement: Transaction boundaries in the service layer
Operations that modify data MUST run within explicit transaction boundaries
defined in the service layer, so a failed multi-step operation does not leave
partial writes.

#### Scenario: Multi-step write fails midway
- GIVEN a service operation performs several writes in one unit of work
- WHEN one of the writes fails
- THEN the whole operation is rolled back
- AND no partial changes are persisted
