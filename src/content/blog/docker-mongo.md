---
author: Vladimir Kutsev
pubDatetime: 2025-02-25T15:45:00Z
modDatetime: 2025-02-25T09:12:47.400Z
title: MongoDB on Docker
slug: mongodb-on-docker
featured: false
image: /public/assets/images/dockerPlusMongo.webp
ogImage: /public/assets/images/dockerPlusMongo.webp
tags:
  - docker
  - mongodb
  - tutorial
description: Quick start guide for running MongoDB on Docker with persistent data storage.
---

![MongoDB on Docker](/public/assets/images/dockerPlusMongo.webp)

This guide will walk you through setting up MongoDB in a Docker container with persistent data storage using Docker volumes. We'll create a dedicated directory structure and configure port mapping for easy access.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Directory Setup](#directory-setup)
- [Docker Configuration](#docker-configuration)
- [Running MongoDB](#running-mongodb)
- [Verifying the Setup](#verifying-the-setup)

## Prerequisites

Before starting, ensure you have:
- Docker installed on your system
- Basic familiarity with terminal/command prompt
- Administrative privileges on your machine

## Directory Setup

First, let's create a dedicated directory structure for our MongoDB instance:

```bash
# Create parent directory
mkdir ~/mongodb-docker

# Create data directory for persistence
mkdir ~/mongodb-docker/data

# Navigate to the MongoDB directory
cd ~/mongodb-docker
```

## Docker Configuration

Create a `docker-compose.yml` file in the mongodb-docker directory:

```yaml
version: '3.8'

services:
  mongodb:
    image: mongo:latest
    container_name: mongodb
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password123
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db
    restart: unless-stopped
volumes:
  mongo-data:
    driver: local
```

Let's break down the configuration:
- `image`: Uses the latest official MongoDB Community Server image
- `ports`: Maps container port 27017 to host port 27017
- `volumes`: Maps the local `./data` directory to MongoDB's `/data/db` directory
- `environment`: Sets up initial root username and password
- `restart`: Automatically restarts the container unless manually stopped

## Running MongoDB

Start the MongoDB container using Docker Compose:

```bash
docker-compose up -d
```

The `-d` flag runs the container in detached mode (background).

## Verifying the Setup

1. Check if the container is running:
```bash
docker ps
```

2. Verify the volume mounting:
```bash
docker volume ls
```

3. Connect to MongoDB using the MongoDB shell:
```bash
docker exec -it mongodb mongosh -u admin -p password123
```

You should now see the MongoDB shell prompt. Try some basic commands:
```javascript
// Show databases
show dbs

// Create a test database
use test_db

// Insert a test document
db.test_db.insertOne({ message: "Hello Docker MongoDB!" })

// Query the document
db.test.find()
```

## Directory Structure
After setup, your directory structure should look like this:
```
~/mongodb-docker/
├── docker-compose.yml
└── data/
    └── ... (MongoDB data files)
```

## Important Notes

1. **Data Persistence**: All your MongoDB data will be stored in the `~/mongodb-docker/data` directory. This ensures your data survives container restarts or removals.

2. **Security**: 
   - Change the default username and password in production
   - Consider adding network restrictions
   - Never expose MongoDB directly to the internet

3. **Backup**: You can backup your data by copying the contents of the data directory. Just ensure MongoDB is stopped first:
```bash
docker-compose down
# Now you can safely backup the data directory
docker-compose up -d
```

Now you have a fully functional MongoDB instance running in Docker with persistent storage! The database will be accessible on `localhost:27017` using the credentials specified in the docker-compose file.
