This project implements a CI/CD pipeline where Jenkins pulls source code from GitHub, builds a Docker image, pushes it to Docker Hub, and deploys the updated container on a Linux deployment server.
Integrated Trivy in Jenkins pipeline to scan Docker images for vulnerabilities before pushing images to Docker Hub.
Stage 1: Code Checkout from GitHub
Stage 2: Docker Image Build
Stage 3: Trivy Vulnerability Scan
Stage 4: Docker Hub Login
Stage 5: Docker Image Push
Stage 6: Deployment on Linux Server
Stage 7: Application Verification