# Albedo Infrastructure

This repository contains the shared Docker Compose setup for local development of Albedo services.

## Services

- **MySQL** (`mysql`): Database used by auth-api and later novels service.
- **Auth API** (`auth-api`): Java Spring Boot authentication service.

## Getting Started

1. **Clone the workspace** using the Albedo manifest (see the `albedo-manifest` repo). This checks out the auth source alongside this repository at `backend/auth/`, which is required by the compose build context.

2. **Environment variables**:  
   Copy `.env.example` to `.env` and adjust values as needed for your local environment.

   ```bash
   cp .env.example .env
   ```

3. **Start the stack**:  
   From the `infrastructure/` directory, run:

   ```bash
   docker compose up -d
   ```

   This will start MySQL and the auth-api service.

4. **Verify**:
   - MySQL should be accessible on `localhost:3306`.
   - Auth API should be accessible on `localhost:8080` (health endpoint `/actuator/health` etc.).

5. **Adding the novels service** (future):
   - Add a new service definition for the novels service in this `docker-compose.yml`.
   - Add any required environment variables to `.env.example`.
   - Ensure the novels service can connect to the MySQL instance via `mysql:3306`.

## Configuration

### MySQL
- `MYSQL_ROOT_PASSWORD`: Root password for MySQL.
- `MYSQL_DATABASE`: Database name created on startup.

### Auth API
- `SPRING_PROFILES_ACTIVE`: Spring profile (default `dev`).
- `SPRING_DATASOURCE_URL`: JDBC URL pointing to the MySQL service.
- `SPRING_DATASOURCE_USERNAME`: Username for MySQL.
- `SPRING_DATASOURCE_PASSWORD`: Password for MySQL.

## Notes

- The auth-api build context is set to `../backend/auth`, meaning the source code is consumed from the sibling `backend/auth` repository. This keeps the Docker Compose file outside of the auth repo, allowing multiple services to share the same infrastructure.
- Compose reads configuration from a local `.env` file when present; `.env.example` provides safe development defaults. Never commit actual passwords.

## License

See the [LICENSE](../LICENSE) file (if applicable) or assume MIT.
