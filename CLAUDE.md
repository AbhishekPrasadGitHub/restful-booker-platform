# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Architecture Overview

Restful-booker-platform is a microservices-based Bed & Breakfast booking system designed for training web service testing and automation. It consists of:

**Backend Services (Java Spring Boot):**
- `auth/` - Authentication service (port 3004)
- `booking/` - Booking CRUD operations (port 3000)
- `room/` - Room management (port 3001)
- `report/` - Reporting service (port 3005)
- `branding/` - Branding/UI customization (port 3002)
- `message/` - Message/communication service (port 3006)

**Frontend:**
- `assets/` - Next.js React application (port 3003) serving the main UI

**Testing:**
- `end-to-end-tests/` - Selenium-based E2E tests with JUnit 5

Each service runs independently and communicates via HTTP APIs. Services log to individual `.log` files in the root directory during local development.

## Common Commands

### Building & Running
```bash
# Full build from scratch (includes dependency checks and E2E tests)
./build_locally.sh

# Run application (after building at least once)
./run_locally.sh

# Run with E2E tests
./run_locally.sh -e true
```

### Individual Service Development
Each backend service (auth, booking, room, etc.):
```bash
# Run tests only
mvn clean test

# Build JAR
mvn clean package

# Run specific service
java -jar target/restful-booker-platform-[service]-*.jar
```

### Frontend (assets/)
```bash
cd assets/
npm run dev        # Development server
npm run build      # Production build
npm run test       # Jest tests
npm run test:watch # Jest in watch mode
npm run lint       # ESLint
```

### End-to-End Tests
```bash
cd end-to-end-tests/
mvn clean test
```

### Docker
```bash
# Run entire platform via Docker Compose
docker-compose up
```

## Development Environment

**Requirements:**
- JDK 21.0.5+ (configured with JAVA_HOME)
- Maven 3.6.3+
- Node 22.14.0
- NPM 10.9.2

**Access Points:**
- Main Application: http://localhost:3003
- Login: admin/password
- Service documentation: http://localhost:[port]/[service]/swagger-ui/index.html
- Health checks: http://localhost:[port]/[service]/actuator/health
- Logs: http://localhost:[port]/[service]/actuator/logfile

## Key Configuration Files

- Root `pom.xml` - Maven multi-module configuration
- `assets/package.json` - Frontend dependencies and scripts with Jest test configuration
- `docker-compose.yml` - Container orchestration for all services
- Individual service `pom.xml` files - Service-specific Maven configurations
- Service READMEs contain specific API documentation and configuration options

## Testing Strategy

- **Unit Tests:** Jest for frontend (assets/), JUnit for backend services
- **Integration Tests:** Each service has Maven-based tests
- **E2E Tests:** Selenium WebDriver tests covering full user workflows
- **API Contract Tests:** JSON contract files in booking/src/test/resources/

Services support development vs production profiles via `-Dspring.profiles.active=dev`.