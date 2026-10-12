# Monorepo with Java 25 and Gradle Kotlin DSL

We decided to structure the project as a monorepo containing `/backend` (Java 25, Spring Boot 3+) and `/frontend` (React, Vite), managed together with a root `docker-compose.yml` for local dependencies (PostgreSQL with PostGIS, Keycloak, MinIO). Gradle with Kotlin DSL is selected as the backend build system to provide concise, type-safe build scripts for modern Spring Boot.
