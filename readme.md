# QR Code Generator - Dockerized Application

A containerized QR code generator that creates QR codes from URLs with customizable colors.

## Features

- Generate QR codes from any URL
- Customizable fill and background colors
- Timestamped output files
- Containerized with Docker
- Automated CI/CD with GitHub Actions

## Quick Start

### Using Docker

#### Pull from DockerHub
```bash
docker pull justincordova/qr-code-generator-app:latest
docker run -d --name qr-generator justincordova/qr-code-generator-app:latest
```

#### Build from Source
```bash
git clone https://github.com/justincordova/is219-docker.git
cd is219-docker
docker build -t qr-code-generator-app .
docker run -d --name qr-generator qr-code-generator-app
```

## Usage

### Default URL (github.com/kaw393939)
```bash
docker run -d --name qr-generator qr-code-generator-app
```

### Custom URL
```bash
docker run -d --name qr-generator \
  qr-code-generator-app --url https://www.njit.edu
```

### With Volume Mount (Persistent Storage)
```bash
docker run -d --name qr-generator \
  -v $(pwd)/my-qr-codes:/app/qr_codes \
  qr-code-generator-app --url https://canvas.njit.edu
```

## Viewing Logs

```bash
docker logs qr-generator
```

## Customizing Colors

Set environment variables for QR code colors:

```bash
docker run -d --name qr-generator \
  -e FILL_COLOR=blue \
  -e BACK_COLOR=yellow \
  qr-code-generator-app --url https://www.njit.edu
```

## DockerHub

Image: `justincordova/qr-code-generator-app:latest`

Repository: https://hub.docker.com/r/justincordova/qr-code-generator-app

## CI/CD

This project uses GitHub Actions for automated building and testing:
- Builds Docker image on every push to main branch
- Runs automated tests
- Pushes to DockerHub on successful builds
- Tags images with git SHA for versioning

**Note:** You must configure GitHub repository secrets for CI/CD to work:
- `DOCKERHUB_USERNAME`: Your DockerHub username
- `DOCKERHUB_TOKEN`: DockerHub access token (create in DockerHub account settings → Security → Access Tokens)

## Security Features

- Runs as non-root user (myuser)
- Minimal base image (python:3.12-slim-bullseye)
- Proper file permissions and ownership
- .dockerignore excludes unnecessary files

## Requirements

- Python 3.12
- qrcode
- python-dotenv
- validators
- Pillow

## Development

### Local Development (without Docker)

```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
python main.py --url https://www.njit.edu
```

## Assignment Submission

This project was completed for IS219 - Spring 2026.

**GitHub Repository:** https://github.com/justincordova/is219-docker

**DockerHub Image:** https://hub.docker.com/r/justincordova/qr-code-generator-app

## License

MIT License - See repository for details
