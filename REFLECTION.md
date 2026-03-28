# Dockerization Reflection - IS219 Assignment

## Experience Overview

This assignment involved containerizing a QR Code Generator application using Docker, setting up continuous integration with GitHub Actions, and deploying to DockerHub.

## Steps Completed

### 1. Environment Setup
- Installed Docker Desktop on Mac
- Verified Docker installation and functionality
- Set up DockerHub account for image storage

### 2. Project Preparation
- Cloned the QR Code Generator repository from GitHub
- Examined main.py to understand application functionality
- Verified and created requirements.txt with all dependencies (qrcode, python-dotenv, validators, Pillow)
- The application generates QR codes from URLs with customizable colors

### 3. Docker Configuration
- Created optimized Dockerfile following security best practices:
  - Used python:3.12-slim-bullseye base image for minimal size
  - Implemented efficient layer caching
  - Created non-root user 'myuser' for security
  - Set up proper directory permissions
- Created .dockerignore to exclude unnecessary files
- Used Docker layer caching by copying requirements.txt first

### 4. Local Testing
- Built Docker image successfully: `docker build -t qr-code-generator-app .`
- Tested default URL generation (github.com/kaw393939)
- Tested custom URL parameter with --url flag
- Tested volume mounting for persistent QR code storage
- Verified QR code files generated correctly as PNG images

### 5. CI/CD with GitHub Actions
- Created .github/workflows/docker.yml workflow
- Configured automatic build on push to main branch
- Set up DockerHub authentication with secrets
- Implemented image versioning with git SHA tags
- Added automated testing step after build

### 6. DockerHub Deployment
- Tagged image with DockerHub username (justincordova/qr-code-generator-app)
- Pushed image to DockerHub registry
- Verified image accessibility and functionality
- Tested pulling and running from DockerHub

## Challenges Faced

### Challenge 1: Understanding Docker Layer Caching
**Issue:** Initially didn't understand why requirements.txt was copied separately.
**Solution:** Learned that Docker caches layers and copying requirements.txt first allows dependency caching, speeding up rebuilds when only code changes.

### Challenge 2: Non-root User Permissions
**Issue:** Application couldn't write to qr_codes directory initially.
**Solution:** Created directories with correct ownership before switching to non-root user, ensuring myuser has write permissions.

### Challenge 3: Missing Pillow Dependency
**Issue:** The qrcode library requires Pillow (PIL) for image generation, but it was missing from the initial requirements.txt.
**Solution:** Added Pillow to requirements.txt and rebuilt the Docker image to include the dependency.

### Challenge 4: DockerHub Image Inconsistency
**Issue:** After fixing the Pillow dependency locally, the DockerHub image still had the old version without Pillow.
**Solution:** Rebuilt the Docker image and pushed it again to DockerHub to ensure the correct dependencies were included.

### Challenge 5: GitHub Actions Secrets
**Issue:** Workflow requires DockerHub credentials but secrets need to be configured manually.
**Solution:** Need to add DOCKERHUB_USERNAME and DOCKERHUB_TOKEN as repository secrets in GitHub settings (user action required).

## Key Learnings

1. **Security Best Practices:** Always run containers as non-root users to minimize attack surface
2. **Image Optimization:** Use slim base images and implement proper layering for smaller, faster images
3. **CI/CD Automation:** GitHub Actions can automate the entire build, test, and deployment pipeline
4. **Volume Persistence:** Understanding volume mounting is crucial for data persistence across container lifecycles
5. **Container Orchestration:** Learned fundamentals of how images are built, tagged, and distributed
6. **Dependency Management:** Properly documenting all dependencies in requirements.txt is critical for reproducible builds

## What Worked Well

- The Dockerfile structure was clear and followed best practices
- Multi-stage builds (well, slim image) kept the final image small (190MB)
- GitHub Actions provided immediate feedback on code changes
- Volume mounting made testing QR code generation straightforward
- Docker layer caching significantly sped up rebuild times

## Areas for Improvement

1. Could add health checks to the Dockerfile for production monitoring
2. Implement more comprehensive automated testing in CI/CD
3. Add environment variable validation for production use
4. Consider implementing multi-architecture builds (ARM64 support)
5. Create proper documentation for setup instructions

## Conclusion

This assignment provided hands-on experience with containerization, CI/CD pipelines, and container registries. The skills learned - building efficient Docker images, automating workflows, and managing container lifecycles - are essential for modern software development and deployment practices.

## Screenshots

### Container Logs Screenshot
[Include screenshot showing: `docker logs qr-generator` with successful QR code generation]

### GitHub Actions Workflow Screenshot
[Include screenshot showing: Successful GitHub Actions run with green checkmark]
