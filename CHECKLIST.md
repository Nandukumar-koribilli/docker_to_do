# 📋 Setup Checklist

Use this checklist to ensure everything is properly configured before deployment.

## ✅ Pre-Deployment Checklist

### 1. Local Environment Setup

- [ ] Docker Desktop installed and running
- [ ] Docker Compose installed (v2.0+)
- [ ] Git installed
- [ ] Node.js installed (v18+) - for local development
- [ ] Python installed (v3.11+) - for local development
- [ ] Code editor (VS Code recommended)

### 2. Project Files Verification

- [ ] All files created successfully
- [ ] `.env` files configured with MongoDB URI
- [ ] `.gitignore` includes `.env` files
- [ ] `docker-compose.yml` present
- [ ] All Dockerfiles present (backend, frontend)
- [ ] GitHub workflows present (main.yml, pr-check.yml, deploy.yml)

### 3. MongoDB Setup

- [ ] MongoDB Atlas account created
- [ ] Cluster created and running
- [ ] Database user created
- [ ] IP whitelist configured (0.0.0.0/0 for development)
- [ ] Connection string obtained
- [ ] Connection string added to `.env` files
- [ ] Connection string tested

### 4. Docker Hub Setup

- [ ] Docker Hub account created
- [ ] Repository created: `fullstack-backend`
- [ ] Repository created: `fullstack-frontend`
- [ ] Access token generated
- [ ] Logged in locally: `docker login`

### 5. GitHub Repository Setup

- [ ] GitHub account ready
- [ ] New repository created
- [ ] Repository is public or private (your choice)
- [ ] Git initialized locally: `git init`
- [ ] Remote added: `git remote add origin <URL>`
- [ ] Initial commit made
- [ ] Code pushed to GitHub

### 6. GitHub Secrets Configuration

Go to: **Repository → Settings → Secrets and variables → Actions**

- [ ] `DOCKER_USERNAME` added
- [ ] `DOCKER_PASSWORD` added (use access token, not password)
- [ ] `MONGODB_URI` added (optional, can use in workflows)

### 7. Local Testing

- [ ] Backend runs locally: `cd backend && python app.py`
- [ ] Frontend runs locally: `cd frontend && npm run dev`
- [ ] Docker Compose works: `docker-compose up -d`
- [ ] Backend health check: `curl http://localhost:5000/health`
- [ ] Frontend accessible: `http://localhost`
- [ ] Can create/read/delete items
- [ ] All services stop cleanly: `docker-compose down`

### 8. Docker Image Testing

- [ ] Backend image builds: `docker build -t test-backend ./backend`
- [ ] Frontend image builds: `docker build -t test-frontend ./frontend`
- [ ] Backend container runs
- [ ] Frontend container runs
- [ ] Images cleaned up after testing

### 9. CI/CD Pipeline Testing

- [ ] Code pushed to GitHub
- [ ] GitHub Actions triggered automatically
- [ ] Backend linting passes
- [ ] Frontend linting passes
- [ ] Docker images build successfully
- [ ] Images pushed to Docker Hub
- [ ] Security scan completes
- [ ] All workflow jobs pass

### 10. Pull Request Testing

- [ ] Feature branch created
- [ ] Changes committed to feature branch
- [ ] Pull request created
- [ ] PR checks run automatically
- [ ] PR comments appear
- [ ] All checks pass

### 11. Deployment Preparation

Choose your deployment platform:

- [ ] AWS ECS configured
- [ ] Azure Container Instances configured
- [ ] Google Cloud Run configured
- [ ] DigitalOcean App Platform configured
- [ ] Heroku configured
- [ ] Self-hosted server configured

### 12. Deployment Configuration

- [ ] Deployment workflow updated (deploy.yml)
- [ ] Cloud provider credentials added to GitHub Secrets
- [ ] Environment variables configured on cloud platform
- [ ] Domain name configured (if applicable)
- [ ] SSL/TLS certificate configured (if applicable)

### 13. Production Deployment

- [ ] Manual deployment workflow triggered
- [ ] Deployment completes successfully
- [ ] Health checks pass
- [ ] Application accessible via public URL
- [ ] All features working
- [ ] Database connection working

### 14. Post-Deployment

- [ ] Monitoring configured
- [ ] Logging configured
- [ ] Alerts configured
- [ ] Backup strategy implemented
- [ ] Documentation updated with production URLs
- [ ] Team notified of deployment

### 15. Security Checklist

- [ ] No secrets in code
- [ ] All secrets in GitHub Secrets or environment variables
- [ ] `.env` files in `.gitignore`
- [ ] Vulnerability scan passing
- [ ] HTTPS enabled in production
- [ ] Security headers configured
- [ ] Database access restricted
- [ ] Non-root users in containers

### 16. Performance Checklist

- [ ] Images optimized (multi-stage builds)
- [ ] Static assets cached
- [ ] Gzip compression enabled
- [ ] Database queries optimized
- [ ] Health checks configured
- [ ] Auto-scaling configured (if needed)

### 17. Documentation Checklist

- [ ] README.md complete
- [ ] DEPLOYMENT.md reviewed
- [ ] TESTING.md reviewed
- [ ] PROJECT_SUMMARY.md reviewed
- [ ] API endpoints documented
- [ ] Environment variables documented
- [ ] Troubleshooting guide reviewed

### 18. Maintenance Checklist

- [ ] Dependency update strategy defined
- [ ] Backup schedule defined
- [ ] Monitoring dashboard set up
- [ ] Incident response plan created
- [ ] Rollback procedure tested
- [ ] Team training completed

---

## 🚀 Quick Start Commands

### First Time Setup

```bash
# 1. Clone/navigate to project
cd "c:\Users\nandu\OneDrive\Desktop\full stack inten\task 3"

# 2. Verify environment files
cat .env

# 3. Start services
docker-compose up -d

# 4. Check status
docker-compose ps

# 5. View logs
docker-compose logs -f

# 6. Test application
curl http://localhost:5000/health
# Open http://localhost in browser

# 7. Stop services
docker-compose down
```

### Push to GitHub

```bash
# 1. Initialize git (if not done)
git init

# 2. Add all files
git add .

# 3. Commit
git commit -m "Initial commit: Full-stack CI/CD pipeline"

# 4. Add remote
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git

# 5. Push
git branch -M main
git push -u origin main
```

### Trigger Deployment

```bash
# 1. Go to GitHub repository
# 2. Click "Actions" tab
# 3. Select "Production Deployment"
# 4. Click "Run workflow"
# 5. Select environment and version
# 6. Click "Run workflow" button
```

---

## 🐛 Common Issues and Solutions

### Issue: Docker Compose fails to start

**Solution**:

- Check Docker Desktop is running
- Run `docker-compose down` first
- Check `.env` file exists and is configured
- Check port 80, 5000, 27017 are not in use

### Issue: MongoDB connection error

**Solution**:

- Verify MongoDB URI in `.env` files
- Check IP whitelist in MongoDB Atlas (add 0.0.0.0/0)
- Ensure network connectivity
- Check MongoDB cluster is running

### Issue: GitHub Actions failing

**Solution**:

- Verify all secrets are set in GitHub
- Check workflow YAML syntax
- Review action logs for specific errors
- Ensure Docker Hub repositories exist

### Issue: Frontend can't connect to backend

**Solution**:

- Check `VITE_API_URL` in frontend `.env`
- Verify backend is running
- Check CORS configuration in backend
- Check network connectivity

### Issue: Docker build fails

**Solution**:

- Check Dockerfile syntax
- Verify all dependencies are listed
- Clear Docker cache: `docker system prune -a`
- Check available disk space

---

## 📊 Success Criteria

Your deployment is successful when:

✅ All checklist items are completed
✅ Local Docker Compose runs without errors
✅ GitHub Actions pipeline passes all stages
✅ Docker images are pushed to Docker Hub
✅ Security scan shows no critical vulnerabilities
✅ Application is accessible via public URL
✅ All CRUD operations work
✅ Health checks return 200 OK
✅ No errors in logs
✅ Team can access and use the application

---

## 🎯 Next Actions

After completing this checklist:

1. **Test thoroughly** - Try all features
2. **Monitor** - Set up monitoring and alerts
3. **Document** - Update any missing documentation
4. **Share** - Share with your team
5. **Iterate** - Gather feedback and improve

---

## 📞 Need Help?

If you encounter issues:

1. Check the documentation:

   - README.md
   - DEPLOYMENT.md
   - TESTING.md
   - PROJECT_SUMMARY.md

2. Review logs:

   - Docker: `docker-compose logs`
   - GitHub Actions: Check Actions tab
   - Application: Check browser console

3. Common resources:
   - Docker documentation
   - GitHub Actions documentation
   - MongoDB Atlas documentation
   - Your cloud provider documentation

---

**Good luck with your deployment! 🚀**
