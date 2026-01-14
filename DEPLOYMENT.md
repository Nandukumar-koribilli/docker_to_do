# CI/CD Deployment Guide

## 📋 Prerequisites Setup

### 1. Docker Hub Account

1. Create account at https://hub.docker.com
2. Create two repositories:
   - `fullstack-backend`
   - `fullstack-frontend`
3. Generate access token: Account Settings → Security → New Access Token

### 2. GitHub Repository Setup

1. Push your code to GitHub
2. Go to: Settings → Secrets and variables → Actions
3. Add the following secrets:

   **Required Secrets:**

   - `DOCKER_USERNAME`: Your Docker Hub username
   - `DOCKER_PASSWORD`: Your Docker Hub access token
   - `MONGODB_URI`: Your MongoDB connection string

   **Optional (for deployment):**

   - `AWS_ACCESS_KEY_ID`: For AWS deployment
   - `AWS_SECRET_ACCESS_KEY`: For AWS deployment
   - `SLACK_WEBHOOK_URL`: For notifications

## 🚀 Deployment Options

### Option 1: AWS ECS (Elastic Container Service)

#### Prerequisites

- AWS Account
- AWS CLI installed
- ECS Cluster created

#### Steps

1. **Create ECS Task Definitions**

   ```bash
   # Create task definition for backend
   aws ecs register-task-definition --cli-input-json file://backend-task-def.json

   # Create task definition for frontend
   aws ecs register-task-definition --cli-input-json file://frontend-task-def.json
   ```

2. **Create ECS Services**

   ```bash
   # Backend service
   aws ecs create-service \
     --cluster your-cluster \
     --service-name backend-service \
     --task-definition backend:1 \
     --desired-count 2

   # Frontend service
   aws ecs create-service \
     --cluster your-cluster \
     --service-name frontend-service \
     --task-definition frontend:1 \
     --desired-count 2
   ```

3. **Update GitHub Workflow**
   Uncomment the AWS deployment section in `.github/workflows/main.yml`

### Option 2: Azure Container Instances

#### Steps

1. **Create Resource Group**

   ```bash
   az group create --name fullstack-rg --location eastus
   ```

2. **Deploy Backend**

   ```bash
   az container create \
     --resource-group fullstack-rg \
     --name backend \
     --image YOUR_USERNAME/fullstack-backend:latest \
     --dns-name-label backend-app \
     --ports 5000 \
     --environment-variables MONGODB_URI=$MONGODB_URI
   ```

3. **Deploy Frontend**
   ```bash
   az container create \
     --resource-group fullstack-rg \
     --name frontend \
     --image YOUR_USERNAME/fullstack-frontend:latest \
     --dns-name-label frontend-app \
     --ports 80
   ```

### Option 3: Google Cloud Run

#### Steps

1. **Enable Cloud Run API**

   ```bash
   gcloud services enable run.googleapis.com
   ```

2. **Deploy Backend**

   ```bash
   gcloud run deploy backend \
     --image YOUR_USERNAME/fullstack-backend:latest \
     --platform managed \
     --region us-central1 \
     --allow-unauthenticated \
     --set-env-vars MONGODB_URI=$MONGODB_URI
   ```

3. **Deploy Frontend**
   ```bash
   gcloud run deploy frontend \
     --image YOUR_USERNAME/fullstack-frontend:latest \
     --platform managed \
     --region us-central1 \
     --allow-unauthenticated
   ```

### Option 4: DigitalOcean App Platform

#### Steps

1. **Install doctl CLI**

   ```bash
   # Install doctl
   # Configure: doctl auth init
   ```

2. **Create app.yaml**

   ```yaml
   name: fullstack-app
   services:
     - name: backend
       image:
         registry_type: DOCKER_HUB
         repository: YOUR_USERNAME/fullstack-backend
         tag: latest
       envs:
         - key: MONGODB_URI
           value: ${MONGODB_URI}
       http_port: 5000

     - name: frontend
       image:
         registry_type: DOCKER_HUB
         repository: YOUR_USERNAME/fullstack-frontend
         tag: latest
       http_port: 80
   ```

3. **Deploy**
   ```bash
   doctl apps create --spec app.yaml
   ```

### Option 5: Heroku

#### Backend Deployment

1. **Create Heroku app**

   ```bash
   heroku create your-backend-app
   ```

2. **Set environment variables**

   ```bash
   heroku config:set MONGODB_URI="your-mongodb-uri" -a your-backend-app
   ```

3. **Deploy**
   ```bash
   heroku container:push web -a your-backend-app
   heroku container:release web -a your-backend-app
   ```

#### Frontend Deployment

1. **Create Heroku app**

   ```bash
   heroku create your-frontend-app
   ```

2. **Deploy**
   ```bash
   heroku container:push web -a your-frontend-app
   heroku container:release web -a your-frontend-app
   ```

### Option 6: Self-Hosted Server (VPS)

#### Prerequisites

- Ubuntu/Debian server
- SSH access
- Docker installed on server

#### Steps

1. **SSH to server**

   ```bash
   ssh user@your-server-ip
   ```

2. **Install Docker and Docker Compose**

   ```bash
   curl -fsSL https://get.docker.com -o get-docker.sh
   sh get-docker.sh
   sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   sudo chmod +x /usr/local/bin/docker-compose
   ```

3. **Clone repository**

   ```bash
   git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
   cd YOUR_REPO
   ```

4. **Set up environment**

   ```bash
   cp .env.example .env
   nano .env  # Edit with your values
   ```

5. **Deploy**

   ```bash
   docker-compose up -d
   ```

6. **Set up Nginx reverse proxy (optional)**

   ```bash
   sudo apt install nginx
   sudo nano /etc/nginx/sites-available/fullstack
   ```

   Add configuration:

   ```nginx
   server {
       listen 80;
       server_name your-domain.com;

       location / {
           proxy_pass http://localhost:80;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
       }

       location /api {
           proxy_pass http://localhost:5000;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
       }
   }
   ```

   Enable and restart:

   ```bash
   sudo ln -s /etc/nginx/sites-available/fullstack /etc/nginx/sites-enabled/
   sudo nginx -t
   sudo systemctl restart nginx
   ```

## 🔄 CI/CD Workflow Usage

### Automatic Deployment (on push to main)

1. **Push code to main branch**

   ```bash
   git add .
   git commit -m "Your commit message"
   git push origin main
   ```

2. **Pipeline automatically runs:**
   - ✅ Lints code
   - ✅ Runs tests
   - ✅ Builds Docker images
   - ✅ Scans for vulnerabilities
   - ✅ Pushes to Docker Hub
   - ✅ Deploys to production

### Manual Deployment

1. **Go to GitHub Actions tab**
2. **Select "Production Deployment" workflow**
3. **Click "Run workflow"**
4. **Select environment and version**
5. **Click "Run workflow" button**

### Pull Request Workflow

1. **Create feature branch**

   ```bash
   git checkout -b feature/new-feature
   ```

2. **Make changes and push**

   ```bash
   git add .
   git commit -m "Add new feature"
   git push origin feature/new-feature
   ```

3. **Create Pull Request on GitHub**
4. **Pipeline automatically:**
   - ✅ Runs linting
   - ✅ Runs tests
   - ✅ Builds Docker images
   - ✅ Comments on PR with results

## 🔍 Monitoring and Verification

### Check Deployment Status

```bash
# Docker Hub
docker pull YOUR_USERNAME/fullstack-backend:latest
docker pull YOUR_USERNAME/fullstack-frontend:latest

# Health checks
curl http://your-backend-url/health
curl http://your-frontend-url/health
```

### View Logs

```bash
# Local Docker Compose
docker-compose logs -f backend
docker-compose logs -f frontend

# AWS ECS
aws ecs describe-tasks --cluster your-cluster --tasks task-id

# Heroku
heroku logs --tail -a your-app-name
```

## 🛠️ Troubleshooting

### Pipeline Fails at Build Stage

- Check Dockerfile syntax
- Verify all dependencies are listed
- Check GitHub Actions logs

### Deployment Fails

- Verify all secrets are set correctly
- Check cloud provider credentials
- Verify service quotas/limits

### Application Not Accessible

- Check firewall rules
- Verify security groups (AWS)
- Check DNS configuration
- Verify environment variables

### Database Connection Issues

- Verify MongoDB URI is correct
- Check IP whitelist in MongoDB Atlas
- Verify network connectivity

## 📊 Monitoring Setup

### Add Application Monitoring

1. **Sentry for Error Tracking**

   ```bash
   # Backend
   pip install sentry-sdk[flask]

   # Frontend
   npm install @sentry/react
   ```

2. **Prometheus for Metrics**
   Add to docker-compose.yml:

   ```yaml
   prometheus:
     image: prom/prometheus
     ports:
       - "9090:9090"
     volumes:
       - ./prometheus.yml:/etc/prometheus/prometheus.yml
   ```

3. **Grafana for Visualization**
   ```yaml
   grafana:
     image: grafana/grafana
     ports:
       - "3001:3000"
   ```

## 🔐 Security Best Practices

1. **Never commit secrets to Git**
2. **Use GitHub Secrets for sensitive data**
3. **Regularly update dependencies**
4. **Enable vulnerability scanning**
5. **Use HTTPS in production**
6. **Implement rate limiting**
7. **Set up WAF (Web Application Firewall)**

## 📝 Versioning Strategy

### Semantic Versioning

```bash
# Major version (breaking changes)
git tag -a v2.0.0 -m "Version 2.0.0"

# Minor version (new features)
git tag -a v1.1.0 -m "Version 1.1.0"

# Patch version (bug fixes)
git tag -a v1.0.1 -m "Version 1.0.1"

# Push tags
git push origin --tags
```

## 🎯 Next Steps

1. ✅ Set up GitHub repository
2. ✅ Configure GitHub Secrets
3. ✅ Choose deployment platform
4. ✅ Configure deployment workflow
5. ✅ Test CI/CD pipeline
6. ✅ Set up monitoring
7. ✅ Configure custom domain
8. ✅ Set up SSL/TLS certificates
9. ✅ Implement backup strategy
10. ✅ Document runbooks

## 📞 Support

For issues or questions:

- Open an issue on GitHub
- Check GitHub Actions logs
- Review cloud provider documentation
- Contact your team lead

---

**Happy Deploying! 🚀**
