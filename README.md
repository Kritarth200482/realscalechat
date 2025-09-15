# RealScaleChat — Scalable Real-Time Chat Platform

> A horizontally-scalable real-time chat application using **Socket.IO + Redis (Pub/Sub)** for low-latency fanout and **Kafka → consumer → PostgreSQL** for durable ingestion.  
> Built as a Turborepo monorepo with a Next.js frontend and a TypeScript Node.js backend.

---

## Features

- Cross-server real-time messaging (no siloed connections).  
- Kafka-backed durable ingestion pipeline.  
- Prisma-based message schema and migrations.  
- Reference `SocketService` (Socket.IO + Redis) and Kafka producer/consumer examples.  
- Patterns and code sketches for rooms, sharding, and per-room ordering.  

---

## Requirements

You’ll need the following installed on your system:

### System dependencies
- **Node.js v18+** (recommended)  
  - macOS (Homebrew + nvm):  
    ```bash
    brew install nvm
    nvm install 18
    nvm use 18
    ```
  - Ubuntu/Debian:  
    ```bash
    curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
    sudo apt-get install -y nodejs
    ```
- **Yarn** (preferred package manager)  
  - macOS: `brew install yarn`  
  - Ubuntu: `npm install -g yarn`  
  - Or enable via Corepack:  
    ```bash
    corepack enable
    corepack prepare yarn@stable --activate
    ```
- **Git**  
  - macOS: `brew install git`  
  - Ubuntu: `sudo apt-get install -y git`
- **Docker & Docker Compose** (recommended for running Redis, Kafka, Postgres locally)  
  - Install Docker Desktop (macOS/Windows) or Docker Engine + docker-compose-plugin (Linux).  
  - Verify installation:  
    ```bash
    docker --version
    docker compose version
    ```

### Project dependencies (installed automatically by Yarn)
When you run `yarn` in the repo root, it will install all workspaces’ dependencies:
- `socket.io` / `socket.io-client`
- `ioredis`
- `kafkajs`
- `prisma` & `@prisma/client`
- `typescript`, `ts-node`, `tsc-watch`
- `next`, `react`, `react-dom`

### Optional tools
- **Redis Insights** (GUI for Redis monitoring)  
- **pgAdmin / psql** (Postgres admin tools)  
- **kcat / kafkacat** (Kafka CLI for topic testing)

---

## Quickstart (Local)

### Option A — Local without Docker (requires external infra)

If you already have Redis, Kafka, and Postgres available (e.g., via **Aiven** or **DigitalOcean Managed Services**):

1. **Clone the repo**
   ```bash
   git clone https://github.com/piyushgarg-dev/Scaleable-WebSockets.git
   cd Scaleable-WebSockets
2.**Install the dependencies**
3. **Set up your Environment variables**
   Create apps/server/.env using your managed service credentials.
   Update .env
   PORT=8000

# Redis
REDIS_HOST=redis-YOURHOST.aivencloud.com
REDIS_PORT=12345
REDIS_USERNAME=default
REDIS_PASSWORD=YOURPASSWORD

# Kafka
KAFKA_BROKERS=kafka-YOURHOST.aivencloud.com:12345
KAFKA_USERNAME=YOURUSER
KAFKA_PASSWORD=YOURPASSWORD
KAFKA_SASL_MECHANISM=plain
KAFKA_CA_PATH=./ca.pem

# Postgres
DATABASE_URL=postgresql://USER:PASSWORD@pg-YOURHOST.aivencloud.com:12345/scalechat?schema=public

4.**Run Prisma migrations**
yarn workspace server prisma generate
yarn workspace server prisma migrate dev --name init

5.**Start apps**
# server
yarn workspace server dev

# web (Next.js frontend)
yarn workspace web dev

