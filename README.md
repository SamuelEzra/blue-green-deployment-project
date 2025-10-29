# Blue-Green Deployment with NGINX

A robust blue-green deployment implementation with automatic failover capabilities using NGINX as a reverse proxy.

## 🚀 Overview

This project implements a blue-green deployment strategy that ensures zero-downtime releases and automatic failover. Two identical application environments (Blue and Green) run simultaneously, with NGINX intelligently routing traffic and providing seamless failover when issues are detected.

## 📋 Features

- **Zero-Downtime Deployments**: Switch between Blue and Green environments without service interruption
- **Automatic Failover**: NGINX automatically routes traffic to healthy environment
- **Header Preservation**: Maintains `X-App-Pool` and `X-Release-Id` headers from applications
- **Chaos Testing**: Built-in endpoints for simulating failures and testing failover
- **Health Monitoring**: Comprehensive health checks for all services
- **Environment Configuration**: Easy switching between environments via environment variables


## 🛠 Prerequisites

- Docker
- Docker Compose
- Git

## 🚀 Quick Start

### 1. Clone and Setup
```bash
git clone <repository-url>
cd blue-green-deployment
```

### 2. Configure the Environment
Define your environment variables in the .env.evample file

### 3. Deploy Services
```bash
docker-compose up -d
```

### 4. Verify Deployment
```bash
# Check all services are running
docker-compose ps

# Test the application
curl http://localhost:8080/version
```

### 5 Testing the Deployment
### Baseline Test (Blue Active)
```bash
curl -v http://localhost:8080/version
```

Expected Response:

    Status: 200 OK

    Headers: X-App-Pool: blue, X-Release-Id: blue-release-1.0.0

### Test Automatic Failover
#### 1. Induce Chaos on Blue Environment
```bash
# Start error mode on Blue
curl -X POST "http://localhost:8081/chaos/start?mode=error"

# Or start timeout mode
curl -X POST "http://localhost:8081/chaos/start?mode=timeout"
```

#### 2. Stop Chaos and Restore Normal Operation
```bash
# Multiple requests to observe failover
curl -X POST http://localhost:8081/chaos/stop
```

