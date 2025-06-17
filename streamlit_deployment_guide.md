# Complete Streamlit Deployment Guide 2025

## Demo App (app.py)
```python
import streamlit as st
import pandas as pd
import numpy as np

st.title("Hello World Streamlit App")
st.write("Welcome to your deployed Streamlit application!")

# Simple interactive elements
name = st.text_input("Enter your name:")
if name:
    st.write(f"Hello, {name}!")

# Sample data visualization
chart_data = pd.DataFrame(
    np.random.randn(20, 3),
    columns=['A', 'B', 'C']
)
st.line_chart(chart_data)

# Sidebar
st.sidebar.header("Options")
option = st.sidebar.selectbox("Choose an option:", ["Option 1", "Option 2", "Option 3"])
st.sidebar.write(f"You selected: {option}")
```

---

## Core Files Required

### requirements.txt
```
streamlit==1.30.0
pandas==2.1.4
numpy==1.24.3
```

### Basic Dockerfile Template
```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8501
CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

---

# DEPLOYMENT PLATFORMS

## 1. Render
**Best for: Reliable hosting with Docker support**

### Files Needed:
- `app.py`
- `requirements.txt`
- `Dockerfile`

### Dockerfile:
```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8501
CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

### Deployment Steps:
1. Push code to GitHub
2. Go to render.com → Create Web Service
3. Connect GitHub repository
4. Select branch and set build/start commands
5. Deploy automatically

### Features:
- Free tier available
- Auto-deploy on Git push
- Built-in SSL
- Environment variables support

---

## 2. Railway
**Best for: Full-stack applications with modern DevOps**

### Files Needed:
- `app.py`
- `requirements.txt`
- `Dockerfile`

### Dockerfile:
```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE $PORT
CMD ["sh", "-c", "streamlit run app.py --server.port=$PORT --server.enableCORS=false --server.address=0.0.0.0"]
```

### Deployment Steps:
1. Push to GitHub
2. Connect Railway to repository
3. Configure environment variables if needed
4. Deploy with one click

### Features:
- Dynamic port handling with $PORT
- Built-in metrics and monitoring
- Database support
- Terraform integration

---

## 3. Streamlit Cloud
**Best for: Beginners and quick prototypes**

### Files Needed:
- `app.py`
- `requirements.txt`
- `.streamlit/config.toml` (optional)

### No Dockerfile Required

### Optional config.toml:
```toml
[theme]
primaryColor = "#FF6B6B"
backgroundColor = "#FFFFFF"
secondaryBackgroundColor = "#F0F2F6"
textColor = "#262730"
```

### Deployment Steps:
1. Push code to GitHub (public repository)
2. Go to streamlit.io/cloud
3. Sign in with GitHub
4. Click "New app"
5. Select repository, branch, and main file
6. Deploy

### Features:
- Completely free
- No Docker knowledge required
- Auto-deploy on Git push
- Limited customization

---

## 4. Heroku
**Best for: Traditional PaaS with extensive add-ons**

### Option A: Docker Deployment
**Files Needed:**
- `app.py`
- `requirements.txt`
- `Dockerfile`

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["streamlit", "run", "app.py", "--server.port=$PORT", "--server.address=0.0.0.0"]
```

**Steps:**
1. Install Heroku CLI
2. `heroku create your-app-name`
3. `heroku container:push web`
4. `heroku container:release web`

### Option B: Buildpack Deployment
**Files Needed:**
- `app.py`
- `requirements.txt`
- `Procfile`
- `runtime.txt`

**Procfile:**
```
web: streamlit run app.py --server.port=$PORT --server.address=0.0.0.0
```

**runtime.txt:**
```
python-3.9.18
```

**Steps:**
1. `git push heroku main`
2. `heroku ps:scale web=1`

### Features:
- Free tier (limited hours)
- Extensive add-ons ecosystem
- Automatic SSL
- Easy scaling

---

## 5. Google Cloud Run
**Best for: Scalable serverless deployment**

### Files Needed:
- `app.py`
- `requirements.txt`
- `Dockerfile`

### Dockerfile:
```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8080
CMD ["streamlit", "run", "app.py", "--server.port=8080", "--server.address=0.0.0.0"]
```

### Deployment Steps:
1. Enable Cloud Run API
2. Build and push to Container Registry:
   ```bash
   gcloud builds submit --tag gcr.io/PROJECT-ID/streamlit-app
   ```
3. Deploy:
   ```bash
   gcloud run deploy --image gcr.io/PROJECT-ID/streamlit-app --platform managed
   ```

### Features:
- Pay-per-use pricing
- Auto-scaling to zero
- Global deployment
- HTTPS by default

---

## 6. AWS (EC2 & Fargate)
**Best for: Enterprise and complex applications**

### Option A: EC2 Deployment
**Files Needed:**
- `app.py`
- `requirements.txt`
- `Dockerfile`

**Steps:**
1. Launch EC2 instance (t2.micro for free tier)
2. Install Docker:
   ```bash
   sudo yum update -y
   sudo yum install docker -y
   sudo service docker start
   ```
3. Build and run:
   ```bash
   docker build -t streamlit-app .
   docker run -p 8501:8501 streamlit-app
   ```
4. Configure security group (port 8501)

### Option B: ECS Fargate
**Steps:**
1. Push image to ECR
2. Create ECS cluster
3. Define task definition
4. Create service with Fargate launch type

### Features:
- Full control over infrastructure
- Extensive AWS services integration
- Auto-scaling capabilities
- Global content delivery

---

## 7. Azure App Service
**Best for: Microsoft ecosystem integration**

### Files Needed:
- `app.py`
- `requirements.txt`
- `Dockerfile`

### Dockerfile:
```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["streamlit", "run", "app.py", "--server.port=8000", "--server.address=0.0.0.0"]
```

### Deployment Steps:
1. Create resource group and App Service plan
2. Deploy using Azure CLI:
   ```bash
   az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name myapp --deployment-container-image-name myregistry.azurecr.io/streamlit-app:latest
   ```

### Features:
- Integration with Azure services
- Built-in authentication
- Scaling options
- Deployment slots for staging

---

## 8. Fly.io
**Best for: Fast global deployment**

### Files Needed:
- `app.py`
- `requirements.txt`
- `Dockerfile`
- `fly.toml`

### fly.toml:
```toml
app = "streamlit-app"
primary_region = "iad"

[[services]]
  internal_port = 8501
  protocol = "tcp"
  auto_stop_machines = true
  auto_start_machines = true

  [[services.ports]]
    port = 80
    handlers = ["http"]

  [[services.ports]]
    port = 443
    handlers = ["tls", "http"]
```

### Deployment Steps:
1. Install Fly CLI
2. `fly launch` (creates fly.toml)
3. `fly deploy`

### Features:
- Global edge deployment
- Automatic HTTPS
- Fast cold starts
- Built-in load balancing

---

## 9. DigitalOcean App Platform
**Best for: Cost-effective cloud deployment**

### Files Needed:
- `app.py`
- `requirements.txt`
- `Dockerfile`

### Deployment Steps:
1. Connect GitHub repository
2. Configure build and run commands
3. Set environment variables
4. Deploy

### Features:
- Predictable pricing
- Built-in CI/CD
- Automatic scaling
- Managed databases

---

## 10. Koyeb
**Best for: Simple containerized deployment**

### Files Needed:
- `app.py`
- `requirements.txt`
- `Dockerfile`

### Deployment Steps:
1. Connect GitHub repository
2. Configure service settings
3. Deploy with auto-scaling

### Features:
- Edge locations
- Auto-scaling
- Free tier available
- Simple pricing model

---

## 11. Hugging Face Spaces
**Best for: ML and data science applications**

### Files Needed:
- `app.py`
- `requirements.txt`
- `README.md` with metadata

### README.md:
```markdown
---
title: My Streamlit App
emoji: 🚀
colorFrom: blue
colorTo: green
sdk: streamlit
sdk_version: 1.30.0
app_file: app.py
pinned: false
---

# My Streamlit Application
Description of your app
```

### Deployment Steps:
1. Create new Space on Hugging Face
2. Select Streamlit SDK
3. Upload files or connect Git repository
4. App deploys automatically

### Features:
- Free hosting
- Perfect for ML demos
- Community visibility
- Easy sharing

---

## 12. Replit
**Best for: Quick prototyping and learning**

### Files Needed:
- `app.py`
- `requirements.txt`
- `.replit` configuration

### .replit:
```
run = "streamlit run app.py --server.port=8080 --server.address=0.0.0.0"

[nix]
channel = "stable-22_11"

[deployment]
run = ["sh", "-c", "streamlit run app.py --server.port=8080 --server.address=0.0.0.0"]
```

### Deployment Steps:
1. Create new Repl
2. Upload files
3. Run to test
4. Deploy using Replit hosting

### Features:
- Browser-based development
- Instant deployment
- Collaborative editing
- Educational focus

---

## 13. Modal
**Best for: Serverless Python applications**

### Files Needed:
- `app.py` (modified for Modal)
- `requirements.txt`

### Modal app.py:
```python
import modal

app = modal.App("streamlit-app")

@app.function(
    image=modal.Image.debian_slim().pip_install(["streamlit", "pandas", "numpy"])
)
@modal.web_endpoint(port=8000)
def run():
    import subprocess
    import os
    
    # Create the streamlit app file
    with open("streamlit_app.py", "w") as f:
        f.write("""
import streamlit as st
import pandas as pd
import numpy as np

st.title("Hello World from Modal!")
st.write("This is running on Modal's serverless platform")

chart_data = pd.DataFrame(
    np.random.randn(20, 3),
    columns=['A', 'B', 'C']
)
st.line_chart(chart_data)
        """)
    
    # Run streamlit
    subprocess.run([
        "streamlit", "run", "streamlit_app.py",
        "--server.port=8000",
        "--server.address=0.0.0.0"
    ])
```

### Deployment Steps:
1. Install Modal CLI: `pip install modal`
2. `modal deploy app.py`

### Features:
- True serverless
- Pay-per-use
- GPU support available
- Python-native

---

## 14. Vercel (Frontend Only)
**Best for: Frontend hosting with iframe embedding**

### Files Needed:
- `index.html`

### index.html:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Streamlit App</title>
    <style>
        body { margin: 0; padding: 0; }
        iframe { width: 100%; height: 100vh; border: none; }
    </style>
</head>
<body>
    <iframe src="YOUR_STREAMLIT_URL_HERE" 
            title="Streamlit Application">
    </iframe>
</body>
</html>
```

### Deployment Steps:
1. Host your Streamlit app on another platform
2. Create Vercel project with index.html
3. Replace YOUR_STREAMLIT_URL_HERE with actual URL
4. Deploy via GitHub or Vercel CLI

### Features:
- Excellent for frontend
- Global CDN
- Preview deployments
- Custom domains

---

## 15. Netlify (Frontend Only)
**Best for: Static site hosting with iframe**

### Files Needed:
- `index.html` (same as Vercel)

### Deployment Steps:
1. Host Streamlit app elsewhere
2. Create Netlify site
3. Upload index.html with iframe
4. Deploy

### Features:
- Drag-and-drop deployment
- Form handling
- Edge functions
- Branch previews

---

## 16. GitHub Pages (Static Only)
**Best for: Documentation and simple frontend**

### Files Needed:
- `index.html` (iframe approach)
- OR GitHub Actions for backend deployment

### GitHub Actions Workflow (.github/workflows/deploy.yml):
```yaml
name: Deploy to External Platform
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to Render
        run: |
          curl -X POST "${{ secrets.RENDER_DEPLOY_HOOK }}"
```

### Features:
- Free static hosting
- Custom domains
- GitHub integration
- Actions for CI/CD

---

# KUBERNETES - ORCHESTRATION FRAMEWORK

## What is Kubernetes?
Kubernetes is a **container orchestration platform**, not a deployment environment. It manages and scales containerized applications across clusters of machines.

## When to Use Kubernetes?
- **High availability** requirements
- **Auto-scaling** needs
- **Multi-service** applications
- **Production-grade** deployments
- **DevOps** automation

## Files Needed:
- `app.py`
- `requirements.txt`
- `Dockerfile`
- `deployment.yaml`
- `service.yaml`
- `ingress.yaml` (optional)

## deployment.yaml:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: streamlit-app
  labels:
    app: streamlit
spec:
  replicas: 3
  selector:
    matchLabels:
      app: streamlit
  template:
    metadata:
      labels:
        app: streamlit
    spec:
      containers:
      - name: streamlit
        image: your-registry/streamlit-app:latest
        ports:
        - containerPort: 8501
        env:
        - name: STREAMLIT_SERVER_PORT
          value: "8501"
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
```

## service.yaml:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: streamlit-service
spec:
  selector:
    app: streamlit
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8501
  type: LoadBalancer
```

## Deployment Steps:
1. Build and push Docker image:
   ```bash
   docker build -t your-registry/streamlit-app:latest .
   docker push your-registry/streamlit-app:latest
   ```

2. Apply Kubernetes manifests:
   ```bash
   kubectl apply -f deployment.yaml
   kubectl apply -f service.yaml
   ```

3. Verify deployment:
   ```bash
   kubectl get pods
   kubectl get services
   ```

4. Access application:
   ```bash
   kubectl get service streamlit-service
   # Use EXTERNAL-IP to access
   ```

## Kubernetes Features:
- **Auto-scaling**: Horizontal Pod Autoscaler
- **Self-healing**: Restart failed containers
- **Load balancing**: Distribute traffic
- **Rolling updates**: Zero-downtime deployments
- **Service discovery**: Internal DNS
- **Config management**: ConfigMaps and Secrets

---

# DEVOPS PROCESSES

## 1. Version Control
### Git Workflow:
```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/username/repo.git
git push -u origin main
```

### Branching Strategy:
- **Main**: Production-ready code
- **Develop**: Integration branch
- **Feature**: Feature development
- **Hotfix**: Emergency fixes

---

## 2. Continuous Integration (CI)

### GitHub Actions Workflow (.github/workflows/ci.yml):
```yaml
name: CI Pipeline
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.9'
          
      - name: Install dependencies
        run: |
          pip install -r requirements.txt
          pip install pytest flake8 bandit pytest-cov
          
      - name: Lint code
        run: flake8 . --max-line-length=88
        
      - name: Security scan
        run: bandit -r . -f json
        
      - name: Run tests
        run: |
          pytest tests/ --cov=app --cov-report=xml
          
      - name: Upload coverage
        uses: codecov/codecov-action@v3
```

---

## 3. Testing Requirements

### Unit Tests (tests/test_app.py):
```python
import pytest
import streamlit as st
from unittest.mock import patch
import sys
import os

# Add the app directory to the path
sys.path.insert(0, os.path.dirname(os.path.dirname(os.path.abspath(__file__))))

def test_app_runs():
    """Test that the app runs without errors"""
    with patch('streamlit.title'), \
         patch('streamlit.write'), \
         patch('streamlit.text_input'), \
         patch('streamlit.line_chart'):
        
        import app  # This should not raise any exceptions

def test_data_processing():
    """Test data processing functions"""
    import pandas as pd
    import numpy as np
    
    # Test data generation
    data = pd.DataFrame(np.random.randn(20, 3), columns=['A', 'B', 'C'])
    assert len(data) == 20
    assert list(data.columns) == ['A', 'B', 'C']
```

### Run Tests:
```bash
pytest tests/ --cov=app --cov-report=html
```

---

## 4. Continuous Deployment (CD)

### GitHub Actions CD (.github/workflows/cd.yml):
```yaml
name: CD Pipeline
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy to Render
        run: |
          curl -X POST "${{ secrets.RENDER_DEPLOY_HOOK }}"
          
      - name: Deploy to Railway
        run: |
          curl -X POST "${{ secrets.RAILWAY_DEPLOY_HOOK }}"
          
      - name: Notify deployment
        run: |
          echo "Deployment completed successfully"
```

---

## 5. Monitoring and Logging

### Application Monitoring:
```python
import logging
import streamlit as st

# Configure logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)
logger = logging.getLogger(__name__)

# Add to your app.py
def log_user_interaction(action, details=None):
    logger.info(f"User action: {action}, Details: {details}")

# Usage
if st.button("Click me"):
    log_user_interaction("button_click", "main_cta")
    st.write("Button clicked!")
```

### Health Check Endpoint:
```python
import streamlit as st
import requests
import time

def health_check():
    """Simple health check function"""
    try:
        # Check if main components are working
        return {
            "status": "healthy",
            "timestamp": time.time(),
            "version": "1.0.0"
        }
    except Exception as e:
        return {
            "status": "unhealthy",
            "error": str(e),
            "timestamp": time.time()
        }
```

---

## 6. Security Best Practices

### Environment Variables:
```python
import os
import streamlit as st

# Load secrets
API_KEY = os.getenv('API_KEY')
DATABASE_URL = os.getenv('DATABASE_URL')

# Streamlit secrets (for Streamlit Cloud)
try:
    API_KEY = st.secrets["API_KEY"]
    DATABASE_URL = st.secrets["DATABASE_URL"]
except:
    pass  # Fall back to environment variables
```

### Security Headers (if using custom server):
```python
from streamlit.web import cli as stcli
import sys

def add_security_headers():
    """Add security headers"""
    # This would be implemented in a custom server wrapper
    pass

if __name__ == '__main__':
    if st._is_running_with_streamlit:
        add_security_headers()
    sys.argv = ["streamlit", "run", "app.py"]
    sys.exit(stcli.main())
```

---

## 7. Performance Optimization

### Caching:
```python
import streamlit as st
import pandas as pd
import time

@st.cache_data
def load_data():
    """Cache expensive data loading operations"""
    time.sleep(2)  # Simulate expensive operation
    return pd.DataFrame({'A': range(1000), 'B': range(1000)})

@st.cache_resource
def init_model():
    """Cache model initialization"""
    # Initialize your ML model here
    return "model_object"

# Usage
data = load_data()
model = init_model()
```

### Optimized Docker Image:
```dockerfile
# Multi-stage build for smaller image
FROM python:3.9-slim as builder

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir --user -r requirements.txt

FROM python:3.9-slim

WORKDIR /app
COPY --from=builder /root/.local /root/.local
COPY . .

# Make sure scripts in .local are usable
ENV PATH=/root/.local/bin:$PATH

EXPOSE 8501

HEALTHCHECK CMD curl --fail http://localhost:8501/_stcore/health

CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

---

## 8. Troubleshooting Common Issues

### Port Configuration Issues:
```bash
# Check what's running on port 8501
lsof -i :8501

# Kill process if needed
kill -9 $(lsof -ti:8501)
```

### Memory Issues:
```python
import psutil
import streamlit as st

def monitor_memory():
    """Monitor memory usage"""
    memory = psutil.virtual_memory()
    st.sidebar.metric("Memory Usage", f"{memory.percent}%")
    
    if memory.percent > 80:
        st.warning("High memory usage detected!")
```

### CORS Issues:
```python
# Add to your Streamlit config
# .streamlit/config.toml
[server]
enableCORS = false
enableXsrfProtection = false
```

### Debugging:
```python
import streamlit as st
import traceback

def debug_info():
    """Show debug information"""
    if st.checkbox("Show Debug Info"):
        st.write("Python version:", sys.version)
        st.write("Streamlit version:", st.__version__)
        st.write("Current working directory:", os.getcwd())
        st.write("Environment variables:", dict(os.environ))

# Add error handling
try:
    # Your app code here
    pass
except Exception as e:
    st.error(f"An error occurred: {str(e)}")
    st.code(traceback.format_exc())
```

---

## Summary

This guide covers 16 deployment platforms plus Kubernetes orchestration, following a logical flow:

1. **Demo App**: Starting point for all deployments
2. **Deployment Platforms**: 16 different hosting options
3. **Kubernetes**: Orchestration framework (separate from hosting)
4. **DevOps**: Post-deployment processes

Choose your platform based on:
- **Beginners**: Streamlit Cloud, Hugging Face Spaces
- **Production**: AWS, Google Cloud, Azure, Kubernetes
- **Cost-effective**: Railway, Render, DigitalOcean
- **Serverless**: Modal, Google Cloud Run
- **Full DevOps**: Kubernetes with proper CI/CD pipeline