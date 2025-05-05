# OpenMRS AppGlobal HIE Integration

This project sets up a Health Information Exchange (HIE) environment using OpenMRS Reference Application 3.x and OpenHIM (Open Health Information Mediator) for interoperability.

## Project Overview

The project consists of two main components:
1. OpenMRS Reference Application 3.x
2. OpenHIM (Open Health Information Mediator)

### OpenMRS Components
- **Gateway**: Acts as a reverse proxy for the OpenMRS application
- **Frontend**: Single Page Application (SPA) for the OpenMRS user interface
- **Backend**: Core OpenMRS server with FHIR capabilities
- **Database**: MariaDB instance for data storage

### OpenHIM Components
- **Core**: The main OpenHIM server for message routing and mediation
- **Console**: Web-based administration interface
- **MongoDB**: Database for OpenHIM
- **Config**: Configuration management for OpenHIM

## Prerequisites

- Docker
- Docker Compose
- Git

## Getting Started

1. Clone the repository and its submodule:
   ```bash
   git clone https://github.com/appglobalmrs/appglobal-hie.git
   cd appglobal-hie
   git submodule update --init --recursive
   ```

2. Start the services:
   ```bash
   docker-compose up --build
   ```

Note: The project uses a git submodule for the OpenMRS Reference Application located at `openmrs-appglobal-refapp`. Make sure to initialize and update the submodule as shown in step 1.

## Accessing Services
### You should be able to acces the AppGlobalMRS ,OpenHIM , OpenCR and Hapi-Fhir instances  at the following urls
| Instance  |     URL       | credentials (user : password)|
|---------- |:-------------:|------:                       |
| OpenHIM   | http://localhost:9000  |  root@openhim.org : openhim |
| OpenCR    | http://localhost:3000/crux  |  root@intrahealth.org  : intrahealth|
| AppGlobalMRS | https://localhost:8089 |    admin : Admin123| 

- OpenHIM Core API: http://localhost:8085

## Configuration

### Environment Variables

The following environment variables can be configured in the `.env` file:

- `TAG`: Docker image tag (default: nightly)
- `COMPOSE_PROJECT_NAME`: Docker Compose project name
- `CLIENTREGISTRY_SERVERURL`: URL for the client registry FHIR endpoint
- `CLIENTREGISTRY_USERNAME`: Username for client registry authentication
- `CLIENTREGISTRY_PASSWORD`: Password for client registry authentication
- `CLIENTREGISTRY_IDENTIFIERROOT`: Root URL for client registry identifiers

## Architecture

The system architecture consists of:

1. **OpenMRS Stack**:
   - Gateway (Nginx) → Frontend (SPA) → Backend (Tomcat) → Database (MariaDB)
   
2. **OpenHIM Stack**:
   - Console → Core → MongoDB

All components are containerized and communicate through a Docker network named `hie`.

## Health Checks

All services include health checks to ensure proper operation:
- OpenMRS Gateway: Basic connectivity check
- OpenMRS Frontend: HTTP endpoint check
- OpenMRS Backend: OpenMRS API endpoint check
- Database: MySQL connection check
- OpenHIM Core: Custom health check script
- OpenHIM Console: HTTP endpoint check

## Maintenance

### Updating Services
To update the services to the latest version:
```bash
docker-compose pull
docker-compose up -d
```

### Updating Git Submodule
To update the OpenMRS Reference Application submodule to the latest version:
```bash
git submodule update --remote
```

### Viewing Logs
To view logs for all services:
```bash
docker-compose logs -f
```

To view logs for a specific service:
```bash
docker-compose logs -f [service-name]
```

## Troubleshooting

Common issues and solutions:

1. **Port Conflicts**: Ensure no other services are using ports 8089, 9000, 8085, or 27018
2. **Database Issues**: Check the MariaDB logs for connection problems
3. **OpenHIM Configuration**: Verify the OpenHIM configuration in the console
4. **Submodule Issues**: If you encounter issues with the submodule, try:
   ```bash
   git submodule update --init --recursive --force
   ```
