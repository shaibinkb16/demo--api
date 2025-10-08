# POSH Training API - Deployment Guide

## Docker Deployment Instructions

### Prerequisites
- Docker installed on the server
- Docker Compose installed (optional but recommended)

### Quick Start with Docker Compose

1. **Clone/Copy the project files to the server**
   ```bash
   # All required files:
   # - Dockerfile
   # - docker-compose.yml
   # - Posh Backend.py
   # - requirements.txt
   # - .env (create from .env.example)
   ```

2. **Create .env file**
   ```bash
   cp .env.example .env
   # Edit .env with production values
   nano .env
   ```

3. **Build and run the container**
   ```bash
   docker-compose up -d
   ```

4. **Check logs**
   ```bash
   docker-compose logs -f
   ```

5. **Stop the container**
   ```bash
   docker-compose down
   ```

### Manual Docker Commands (without Docker Compose)

1. **Build the Docker image**
   ```bash
   docker build -t posh-training-api .
   ```

2. **Run the container**
   ```bash
   docker run -d \
     --name posh-training-api \
     -p 8080:8080 \
     --env-file .env \
     posh-training-api
   ```

3. **View logs**
   ```bash
   docker logs -f posh-training-api
   ```

4. **Stop the container**
   ```bash
   docker stop posh-training-api
   docker rm posh-training-api
   ```

### Environment Variables

Required variables in `.env`:
- `MONGO_URI` - MongoDB connection string
- `SECRET_KEY` - JWT secret key
- `ALGORITHM` - JWT algorithm (default: HS256)
- `ACCESS_TOKEN_EXPIRE_MINUTES` - Token expiry time (default: 30)
- `ALLOWED_ORIGINS` - Comma-separated CORS origins
- `HOST` - Server host (default: 0.0.0.0)
- `PORT` - Server port (default: 8080)

### Accessing the API

Once deployed, the API will be available at:
- `http://<server-ip>:8080`

Health check endpoint:
- `http://<server-ip>:8080/health`

### Updating the Application

1. **Pull latest changes**
2. **Rebuild and restart**
   ```bash
   docker-compose down
   docker-compose up -d --build
   ```

### Troubleshooting

**Check container status:**
```bash
docker ps -a
```

**Check logs:**
```bash
docker logs posh-training-api
```

**Access container shell:**
```bash
docker exec -it posh-training-api /bin/bash
```

**Remove all and restart:**
```bash
docker-compose down
docker-compose up -d --build --force-recreate
```

### Production Recommendations

1. **Use a reverse proxy** (nginx/traefik) for HTTPS
2. **Set up monitoring** (Prometheus, Grafana)
3. **Configure log rotation**
4. **Use Docker secrets** for sensitive environment variables
5. **Set resource limits** in docker-compose.yml:
   ```yaml
   deploy:
     resources:
       limits:
         cpus: '1'
         memory: 512M
   ```

### Sharing the Container

**Option 1: Share as Docker Image**
```bash
# Save image to tar file
docker save posh-training-api > posh-training-api.tar

# On target server, load the image
docker load < posh-training-api.tar
```

**Option 2: Push to Docker Registry**
```bash
# Tag image
docker tag posh-training-api your-registry/posh-training-api:v1.0

# Push to registry
docker push your-registry/posh-training-api:v1.0
```

**Option 3: Share project files** (Recommended)
- Share entire project directory
- IT team builds locally using Dockerfile
