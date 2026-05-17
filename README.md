# 🚀 FastAPI Deployment on Google Cloud Run

---

# 📌 Introduction

This document explains how to deploy a FastAPI backend application using:

- Python
- FastAPI
- Docker
- Google Cloud Run
- Google Cloud Shell

The deployment is completely serverless.

Google Cloud automatically:

- Builds Docker image
- Runs container
- Deploys application
- Creates HTTPS URL
- Handles scaling

---

# 🧠 What is Cloud Run?

Google Cloud Run is a serverless container platform.

It runs applications inside Docker containers.

Developers only provide:

- Source code
- Dockerfile
- requirements.txt

Cloud Run automatically:

- Builds container
- Deploys application
- Runs application
- Manages infrastructure

---

# 📌 Deployment Architecture

```
FastAPI Code
      ↓
requirements.txt
      ↓
Dockerfile
      ↓
Cloud Build
      ↓
Docker Container
      ↓
Cloud Run
      ↓
Public HTTPS URL
```

---

# 📁 Project Structure

```
fastapi-app/
│
├── main.py
├── requirements.txt
└── Dockerfile
```

---

# 📄 Step 1 — Create FastAPI Application

## Create `main.py`

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def home():
    return {"message": "Cloud Run Success"}
```

---

# 📄 Step 2 — Create requirements.txt

## Create `requirements.txt`

```
fastapi
uvicorn
```

---

# 📄 Step 3 — Create Dockerfile

## Create file named:

```
Dockerfile
```

## Dockerfile Content

```docker
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8080"]
```

---

# 🧠 Understanding Dockerfile

## 1️⃣ Base Image

```docker
FROM python:3.11-slim
```

Uses Python 3.11 environment.

---

## 2️⃣ Working Directory

```docker
WORKDIR /app
```

Creates application folder inside container.

---

## 3️⃣ Copy requirements.txt

```docker
COPY requirements.txt .
```

Copies requirements file into container.

---

## 4️⃣ Install Packages

```docker
RUN pip install --no-cache-dir -r requirements.txt
```

Installs Python libraries.

---

## 5️⃣ Copy Application Files

```docker
COPY . .
```

Copies project files.

---

## 6️⃣ Run FastAPI

```docker
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8080"]
```

Starts FastAPI application.

---

# 📌 Difference Between requirements.txt and Dockerfile

| requirements.txt | Dockerfile |
| --- | --- |
| Contains Python packages | Contains full container instructions |
| Used by pip | Used by Docker |
| Dependency management | Container configuration |
| Only package list | Complete runtime environment |

---

# 🚀 Why Dockerfile is Needed

Cloud Run runs Docker containers.

Dockerfile tells Cloud Run:

- Which Python version to use
- Which packages to install
- Which files to copy
- Which command to run
- Which port to expose

Without Dockerfile, Cloud Run cannot correctly build the container.

---

# ☁️ Step 4 — Open Google Cloud Console

Open:

https://console.cloud.google.com/

---

# ☁️ Step 5 — Open Cloud Shell

Click:

```
Activate Cloud Shell
```

Cloud Shell terminal opens.

---

# ☁️ Step 6 — Create Project Folder

```bash
mkdir fastapi-app
```

Move into folder:

```bash
cd fastapi-app
```

---

# ☁️ Step 7 — Upload Files

Upload these files:

- main.py
- requirements.txt
- Dockerfile

Using:

```
⋮ → Upload
```

---

# ☁️ Step 8 — Verify Files

```bash
ls
```

Expected Output:

```
Dockerfile
main.py
requirements.txt
```

---

# ☁️ Step 9 — Configure Google Cloud Project

## Set Project

```bash
gcloud config set project PROJECT_ID
```

Example:

```bash
gcloud config set project backendapi-496502
```

---

# ☁️ Step 10 — Enable Required APIs

## Enable Cloud Run

```bash
gcloud services enable run.googleapis.com
```

## Enable Cloud Build

```bash
gcloud services enable cloudbuild.googleapis.com
```

---

# ☁️ Step 11 — Deploy to Cloud Run

Run:

```bash
gcloud run deploy fastapi-service \
--source . \
--platform managed \
--region asia-south1 \
--allow-unauthenticated
```

---

# 🧠 What Happens Internally?

Cloud Run automatically:

1. Detects Dockerfile
2. Builds Docker image
3. Creates container
4. Runs FastAPI app
5. Creates public URL

Equivalent local Docker commands:

```bash
docker build
```

```bash
docker run
```

Cloud Run performs these automatically.

---

# 🎉 Successful Deployment Output

Example:

```
Service URL: https://fastapi-service-xxxxx.a.run.app
```

---

# 📌 Open Application

## API URL

```
https://your-service-url.run.app
```

## Swagger Documentation

```
https://your-service-url.run.app/docs
```

---

# 📌 How to Check Cloud Run Service

## List Services

```bash
gcloud run services list
```

---

## Read Logs

```bash
gcloud run services logs read fastapi-service --region asia-south1
```

---

## Describe Service

```bash
gcloud run services describe fastapi-service --region asia-south1
```

---

# 📌 Redeploy After Code Changes

After updating code:

```bash
gcloud run deploy fastapi-service \
--source . \
--platform managed \
--region asia-south1 \
--allow-unauthenticated
```

---

# 📌 Delete Cloud Run Service

```bash
gcloud run services delete fastapi-service --region asia-south1
```

---

# 🔥 Common Errors and Solutions

---

## ❌ Dockerfile Not Found

### Reason

Dockerfile missing or wrong filename.

### Fix

File name must be exactly:

```
Dockerfile
```

---

## ❌ Container Failed to Start

### Reason

Wrong port.

### Fix

Use:

```docker
--port 8080
```

---

## ❌ Uvicorn Not Found

### Reason

uvicorn missing in requirements.txt.

### Fix

Add:

```
uvicorn
```

---

## ❌ Build Failed

### Reason

Files not uploaded correctly.

### Fix

Verify:

```bash
ls
```

Expected:

```
Dockerfile
main.py
requirements.txt
```

---

# 📌 Advantages of Cloud Run

| Feature | Benefit |
| --- | --- |
| Serverless | No server management |
| Auto Scaling | Automatic scaling |
| HTTPS | Automatic SSL |
| Docker Support | Easy deployment |
| Pay Per Use | Low cost |
| Fast Deployment | Easy updates |

---

# 📌 Advantages of Docker

| Benefit | Description |
| --- | --- |
| Same Environment | Works everywhere |
| Isolation | Separate dependencies |
| Portability | Easy migration |
| Consistency | Same in local and cloud |
| Scalability | Easy container scaling |

---

# 📌 Local Docker vs Cloud Run

| Local Docker | Cloud Run |
| --- | --- |
| Manual docker build | Automatic build |
| Manual docker run | Automatic container run |
| Local machine | Google servers |
| Manual scaling | Auto scaling |

---

# 🎯 Final Understanding

## requirements.txt

Used for:

```
Python dependency management
```

---

## Dockerfile

Used for:

```
Container creation and application execution
```

---

## Cloud Run

Used for:

```
Automatic container deployment and hosting
```

---

# ✅ Final Summary

Cloud Run deployment process:

```
Create FastAPI App
        ↓
Create requirements.txt
        ↓
Create Dockerfile
        ↓
Upload Files to Cloud Shell
        ↓
Run gcloud deploy command
        ↓
Cloud Run builds Docker container
        ↓
Application deployed successfully
```

---

# 🎉 Congratulations

Successfully deployed FastAPI backend using:

- FastAPI
- Docker
- Google Cloud Run
- Google Cloud Shell

This is a real-world cloud deployment workflow used in modern backend development.
