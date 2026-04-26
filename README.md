# Invoice System – Swarm Deployment

Docker Swarm stack configuration for the Invoice System microservices application.

## Prerequisites

- Docker with Swarm mode enabled
- 1 Manager node + 2 Worker nodes
- Docker Hub images built and pushed (see CI/CD section)

## Quick Start

```bash
# 1. Initialize Swarm on manager node
docker swarm init

# 2. Join workers (for each worker)
docker swarm join --token <token> <manager-ip>:2377

# 3. Deploy the stack
docker stack deploy -c docker-compose.yml invoice-app

# 4. Check services
docker stack services invoice-app
docker stack ps invoice-app
```

## Services & Ports

| Service       | Port  | Description                  |
|---------------|-------|------------------------------|
| Kong Gateway  | 80    | Public API entry point       |
| Kong Admin    | 8001  | Kong admin API               |
| Portainer     | 9000  | Swarm management UI          |
| Adminer       | 8080  | Database management UI       |
| Kibana        | 5601  | Logging dashboard            |

## API Routes (via Kong on port 80)

| Route                  | Service   |
|------------------------|-----------|
| /api/auth/register     | Auth      |
| /api/auth/login        | Auth      |
| /api/auth/validate     | Auth      |
| /api/customers         | Customers |
| /api/products          | Products  |
| /api/invoices          | Invoices  |

## Networks

| Network             | Purpose                                    |
|---------------------|--------------------------------------------|
| public-net          | Kong, Portainer, Kibana, Adminer           |
| internal-services   | Microservices inter-communication (isolated)|
| db-net              | PostgreSQL + microservices only (isolated) |
| logging-net         | ELK Stack (isolated)                       |
| portainer-agent-net | Portainer agent communication              |

## Remove stack

```bash
docker stack rm invoice-app
```
