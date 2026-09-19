# M-Pesa STK

A Spring Boot application packaged as a containerized local environment with PostgreSQL and Nginx.

## Stack

- Java 21
- Spring Boot 4.1.1
- Maven
- PostgreSQL 16
- Nginx 1.27
- Docker Compose

## Prerequisites

Install the following tools:

- Docker Desktop with Docker Compose
- Git, if cloning the repository

## Run Locally

1. Create a local environment file from the example:

   ```powershell
   Copy-Item .env.example .env
   ```

   On macOS or Linux:

   ```bash
   cp .env.example .env
   ```

2. Update `.env` with local credentials if needed. The file is ignored by Git and must not contain production secrets committed to the repository.

3. Build and start the complete environment:

   ```bash
   docker compose up --build
   ```

   The application is available through Nginx at `http://localhost` by default. Set `NGINX_PORT` in `.env` to use another host port.

## Services

| Service | Purpose | Internal port |
| --- | --- | --- |
| `app` | Spring Boot application | `8080` |
| `db` | PostgreSQL database | `5432` |
| `nginx` | Reverse proxy and public entry point | `80` |

The application waits for PostgreSQL to pass its health check before starting. Database data is stored in the named `postgres-data` volume.

## Environment Variables

| Variable | Description | Example |
| --- | --- | --- |
| `POSTGRES_DB` | Database name | `mpesa` |
| `POSTGRES_USER` | Database username | `mpesa` |
| `POSTGRES_PASSWORD` | Database password | `change-me` |
| `NGINX_PORT` | Host port mapped to Nginx | `80` |

## Useful Commands

View service logs:

```bash
docker compose logs -f
```

Stop the environment:

```bash
docker compose down
```

Stop the environment and remove the database volume:

```bash
docker compose down -v
```

Rebuild the application image:

```bash
docker compose build --no-cache app
```

## Local Maven Build

To build or test without Docker, use the Maven wrapper:

```bash
./mvnw test
./mvnw package
```

On Windows PowerShell:

```powershell
.\mvnw.cmd test
.\mvnw.cmd package
```

## Project Structure

```text
.
├── Dockerfile
├── docker-compose.yml
├── nginx/
│   └── nginx.conf
├── src/
│   ├── main/
│   └── test/
├── pom.xml
└── .env.example
```
