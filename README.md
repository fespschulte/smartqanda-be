# Smart Q & A

A robust, type-safe, and scalable backend service for managing Q&A rooms, built with modern TypeScript, Fastify, Drizzle ORM, and PostgreSQL. Designed for best engineering standards, maintainability, and developer experience.

---

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Tech Stack & Libraries](#tech-stack--libraries)
- [Database](#database)
- [API Endpoints](#api-endpoints)
- [Development Workflow](#development-workflow)
- [Environment Variables](#environment-variables)
- [Linting & Formatting](#linting--formatting)
- [Contribution](#contribution)

---

## Overview

Smart Q & A is a backend service for managing Q&A rooms, providing a clean API and leveraging modern TypeScript features, static validation, and a fully typed database layer. The project is engineered for clarity, extensibility, and production-readiness.

## Architecture

- **Fastify** is used as the HTTP server for its speed, extensibility, and type safety.
- **Drizzle ORM** provides a fully typed, migration-safe interface to PostgreSQL, ensuring schema consistency and developer productivity.
- **Zod** is used for runtime validation and type inference, ensuring all inputs and environment variables are validated.
- **TypeScript** is used throughout for static type safety and maintainability.
- **Docker** and **docker-compose** are used for local development and database orchestration.
- **Biome** is used for linting and formatting, enforcing code quality and consistency.

## Tech Stack & Libraries

| Library/Tool                                                                      | Purpose                                                       |
| --------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| [Fastify](https://fastify.dev/)                                                   | High-performance HTTP server with TypeScript support          |
| [@fastify/cors](https://github.com/fastify/fastify-cors)                          | CORS middleware for Fastify                                   |
| [fastify-type-provider-zod](https://github.com/fastify/fastify-type-provider-zod) | Zod integration for Fastify route validation and typing       |
| [Drizzle ORM](https://orm.drizzle.team/)                                          | Type-safe ORM for SQL databases (PostgreSQL)                  |
| [drizzle-kit](https://orm.drizzle.team/docs/overview)                             | Migration and schema generation tool for Drizzle ORM          |
| [drizzle-seed](https://github.com/nyinyithann/drizzle-seed)                       | Type-safe database seeding                                    |
| [postgres](https://github.com/porsager/postgres)                                  | PostgreSQL client for Node.js                                 |
| [Zod](https://zod.dev/)                                                           | TypeScript-first schema validation with static type inference |
| [TypeScript](https://www.typescriptlang.org/)                                     | Static typing for JavaScript                                  |
| [Biome](https://biomejs.dev/)                                                     | Linting and formatting for code quality                       |
| [Docker](https://www.docker.com/)                                                 | Containerization and local development environment            |

## Database

- **PostgreSQL** is used as the primary database, orchestrated via Docker.
- **Drizzle ORM** defines the schema in `src/db/schema/rooms.ts` and manages migrations in `src/db/migrations/`.
- **Migrations** are generated and run using `drizzle-kit`.
- **Seeding** is handled by `drizzle-seed` for development/testing.
- **Vector Extension**: The database is initialized with the `vector` extension (see `docker/setup.sql`).

### Example Table: `rooms`

| Column      | Type      | Constraints        |
| ----------- | --------- | ------------------ |
| id          | uuid      | PK, default random |
| name        | text      | not null           |
| description | text      | nullable           |
| created_at  | timestamp | not null, default  |

## API Endpoints

| Method | Endpoint  | Description    |
| ------ | --------- | -------------- |
| GET    | `/rooms`  | List all rooms |
| GET    | `/health` | Health check   |

Example usage (see `client.http`):

```http
GET http://localhost:3333/rooms
GET http://localhost:3333/health
```

## Development Workflow

### Prerequisites

- **Node.js** (v18 or newer recommended)
- **npm** (comes with Node.js)
- **Docker** & **Docker Compose** (for local PostgreSQL)

### Setup Instructions

1. **Clone the repository**

   ```sh
   git clone <REPO_URL>
   cd <project-root>
   ```

2. **Install dependencies**

   ```sh
   npm install
   ```

3. **Copy and configure environment variables**

   - Create a `.env` file in the project root:
     ```sh
     cp .env.example .env
     ```
   - Edit `.env` and set the correct `DATABASE_URL` and `PORT` if needed. Example:
     ```env
     PORT=3333
     DATABASE_URL=postgresql://docker:senha_segura@localhost:5432/agents
     ```

4. **Start the PostgreSQL database using Docker Compose**

   ```sh
   docker-compose up -d
   ```

   - This will run a local PostgreSQL instance with the required vector extension.
   - If you encounter port conflicts, ensure port 5432 is free or update the mapping in `docker-compose.yml`.

5. **Run database migrations**

   ```sh
   npm run db:migrate
   ```

   - This will apply all pending migrations to your local database.

6. **(Optional) Seed the database with sample data**

   ```sh
   npm run db:seed
   ```

   - This will populate the `rooms` table with sample data for development/testing.

7. **Start the development server**

   ```sh
   npm run dev
   ```

   - The server will start on the port specified in your `.env` (default: 3333).

8. **Test the API**
   - Use the provided `client.http` file with an HTTP client (like VSCode REST Client) or run:
     ```sh
     curl http://localhost:3333/health
     curl http://localhost:3333/rooms
     ```

#### Troubleshooting

- If you see connection errors, ensure Docker is running and the database container is healthy.
- If migrations fail, check your `DATABASE_URL` and database logs.
- For any issues, check logs in the terminal and ensure all dependencies are installed.

## Environment Variables

All environment variables are validated at runtime using Zod. Example:

| Variable     | Description               | Example Value                                          |
| ------------ | ------------------------- | ------------------------------------------------------ |
| PORT         | Server port               | 3333                                                   |
| DATABASE_URL | PostgreSQL connection URL | postgresql://docker:senha_segura@localhost:5432/agents |

## Linting & Formatting

- **Biome** is used for linting and formatting. Configuration is in `biome.jsonc`.
- To lint/format code:
  ```sh
  npx biome check .
  npx biome format .
  ```

## Contribution

- Follow the existing code style enforced by Biome.
- Ensure all new code is fully typed and validated.
- Write clear, maintainable, and well-documented code.
- Use descriptive commit messages (gitmoji convention is encouraged).

---

**Engineering Notes:**

- The project emphasizes type safety, runtime validation, and reproducible environments.
- All dependencies are chosen for their type safety, performance, and maintainability.
- The codebase is structured for extensibility and clarity, with a focus on best practices for modern TypeScript backends.
