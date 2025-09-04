# Flask CI/CD Pipeline

A simple Flask app with a CI/CD pipeline using Jenkins, Docker, GitHub, and AWS EC2.

## Setup
1. Clone the repo: `git clone https://github.com/yourusername/flask-cicd.git`
2. Install dependencies: `pip install -r requirements.txt`
3. Run locally: `python app.py`
4. Build Docker image: `docker build -t flask-app:latest .`
5. Run Docker container: `docker run -p 5000:5000 flask-app:latest`

## CI/CD Pipeline
- Jenkins pulls code from GitHub.
- Builds and tests the app.
- Creates a Docker image and pushes to Docker Hub.
- Deploys to an EC2 instance via SSH.

## Requirements
- AWS EC2 instance with Elastic IP
- Jenkins server with Docker installed
- GitHub and Docker Hub accounts
