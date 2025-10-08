# POSH Training API

FastAPI backend for POSH Training system with quiz functionality.

## Features

- User authentication with JWT tokens
- Email authorization system
- Training progress tracking
- Quiz score management
- Leaderboard functionality
- CORS support for Unity WebGL builds

## Prerequisites

- Docker and Docker Compose (recommended)
- OR Python 3.10+
- MongoDB Atlas account

## Quick Start with Docker

1. **Clone the repository**
   ```bash
   cd "API"
   ```

2. **Create `.env` file** (copy from `.env.example` if provided)
   ```env
   MONGO_URI=your_mongodb_connection_string
   SECRET_KEY=your_secret_key_here
   ALGORITHM=HS256
   ACCESS_TOKEN_EXPIRE_MINUTES=30
   ALLOWED_ORIGINS=http://localhost:3000,https://your-s3-bucket.s3.region.amazonaws.com
   HOST=0.0.0.0
   PORT=8080
   ```

3. **Build and run with Docker Compose**
   ```bash
   docker-compose up -d
   ```

4. **Check API health**
   ```bash
   curl http://localhost:8080/health
   ```

## Manual Setup (Without Docker)

1. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

2. **Create `.env` file** (as shown above)

3. **Run the application**
   ```bash
   python "Posh Backend.py"
   ```

## API Endpoints

### Authentication
- `POST /auth` - Authenticate user with email
- `GET /check-email/{email}` - Check if email is authorized

### Training Progress
- `POST /progress/start` - Start tracking a slide
- `POST /progress/end` - End tracking a slide
- `POST /progress/finish` - Mark training as completed
- `GET /progress` - Get user progress

### Quiz
- `POST /quiz/submit` - Submit quiz score
- `GET /quiz/score` - Get user's quiz score
- `GET /quiz/leaderboard?limit=10` - Get top scores

### Health Check
- `GET /health` - API health status

## Docker Commands

```bash
# Build and start
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down

# Rebuild after code changes
docker-compose up -d --build

# Remove everything including volumes
docker-compose down -v
```

## Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| MONGO_URI | MongoDB connection string | Required |
| SECRET_KEY | JWT secret key | Required |
| ALGORITHM | JWT algorithm | HS256 |
| ACCESS_TOKEN_EXPIRE_MINUTES | Token expiration time | 30 |
| ALLOWED_ORIGINS | Comma-separated CORS origins | localhost |
| HOST | Server host | 0.0.0.0 |
| PORT | Server port | 8080 |

## Security Notes

1. **Never commit `.env` file** - Contains sensitive credentials
2. **Use strong SECRET_KEY** - Generate with: `openssl rand -hex 32`
3. **Restrict CORS origins** - Only allow trusted domains in production
4. **Use HTTPS** - Always use SSL/TLS in production
5. **MongoDB Security** - Use MongoDB Atlas with IP whitelist and strong passwords

## Deployment

### AWS EC2 / VPS
1. Install Docker and Docker Compose
2. Upload project files
3. Create `.env` file with production credentials
4. Run `docker-compose up -d`
5. Configure reverse proxy (Nginx) with SSL

### Render / Railway / Fly.io
1. Connect GitHub repository
2. Set environment variables in dashboard
3. Deploy automatically

## S3 CORS Configuration

Add this to your S3 bucket CORS configuration:

```json
[
    {
        "AllowedHeaders": ["*"],
        "AllowedMethods": ["GET", "HEAD"],
        "AllowedOrigins": ["*"],
        "ExposeHeaders": ["Content-Length", "Content-Type", "Content-Encoding"],
        "MaxAgeSeconds": 3600
    }
]
```

## MongoDB Collections

### `users`
Stores user data, progress, and quiz scores.

### `authorized_emails`
Contains emails authorized to access the system.

## Support

For issues or questions, contact the development team.
