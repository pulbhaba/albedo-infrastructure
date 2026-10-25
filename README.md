# Albedo Infrastructure

This repository contains the shared Docker Compose setup for local development
of Albedo services. It is a local development stack, not production AWS
provisioning.

## Services

- **MySQL** (`mysql`): Database used by auth-api and novels-api.
- **Auth API** (`auth-api`): Java Spring Boot authentication service.
- **Novels API** (`novels-api`): Local FastAPI server built from
  `../backend/novels`.

## Getting Started

1. Clone the workspace using the Albedo manifest. The auth and novels source
   repositories must be checked out at `backend/auth/` and `backend/novels/`.
2. Copy `.env.example` to `.env` and adjust values for your local environment:

   ```bash
   cp .env.example .env
   ```

3. Start the stack from this directory:

   ```bash
   docker compose up --build -d
   ```

4. Verify the services:
   - MySQL is available on `localhost:3306`.
   - Auth API is available on `localhost:8080`.
   - Novels API is available on `localhost:8000`; its unauthenticated health
     endpoint is `http://localhost:8000/health`.

## Configuration

### MySQL

- `MYSQL_ROOT_PASSWORD`: Root password for MySQL.
- `MYSQL_DATABASE`: Database created at startup.

### Auth API

- `SPRING_PROFILES_ACTIVE`: Spring profile (default `dev`).
- `SPRING_DATASOURCE_URL`: JDBC URL pointing to MySQL.
- `SPRING_DATASOURCE_USERNAME`: MySQL username.
- `SPRING_DATASOURCE_PASSWORD`: MySQL password.

### Novels API

- `NOVELS_HOST_PORT` / `NOVELS_CONTAINER_PORT`: Host and container ports for
  the local HTTP server (default `8000`).
- `AUTH_ISSUER`, `AUTH_AUDIENCE`, and `AUTH_JWKS_URL`: JWT verification
  settings. The defaults point to the local auth-api container.
- `NOVELS_CHAPTER_CONTENT_DIR`: Container directory for chapter version files.
  Compose mounts the named `novels-chapter-content` volume there, so local
  chapter bodies survive novels-api container replacement. Remove the data
  intentionally with `docker compose down --volumes`.

## Notes

- The auth-api and novels-api build contexts use sibling repositories at
  `../backend/auth` and `../backend/novels`. This keeps the Compose file outside
  the service repositories.
- Compose reads a local `.env` file when present. `.env.example` provides safe
  development defaults; never commit actual passwords.
- This stack does not provision an AWS S3 bucket or Lambda execution role. The
  Lambda composition uses S3 for chapter content; this Compose stack uses the
  filesystem-backed adapter on the named local volume.

## License

See the [LICENSE](../LICENSE) file (if applicable) or assume MIT.
