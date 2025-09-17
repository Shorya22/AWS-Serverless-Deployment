# AWS Serverless Deployment (Python 3.12 + Docker + Serverless Framework)

This project provides a **Standard Operating Procedure (SOP)** for deploying a Python 3.12 AWS Lambda function using **Docker**, the **Serverless Framework**, and **API Gateway** on a **Windows 11 (x86_64)** machine.  
It also includes solutions for common issues such as image manifest incompatibility when building Lambda Docker images.

---

## 📌 Document Purpose
Provide a repeatable guide for deploying the `my-app-test` serverless application with Dockerized AWS Lambda using the Serverless Framework.

---

## ⚡ Prerequisites
Make sure you have the following installed and configured:

- AWS Account with CLI credentials configured  
- [AWS CLI](https://docs.aws.amazon.com/cli/)  
- [Node.js (LTS) & npm](https://nodejs.org/)  
- [Serverless Framework](https://www.serverless.com/) → install via `npm install -g serverless`  
- [Docker Desktop with Buildx](https://docs.docker.com/build/buildx/)  
- [WSL2 enabled](https://learn.microsoft.com/en-us/windows/wsl/)  
- [Python 3.12](https://www.python.org/downloads/)  

---

## 📂 Project Structure
```
D:\AWS\app2
├── Dockerfile
├── main.py
├── requirements.txt
└── serverless.yml
```

---

## 📝 Files Overview

**main.py**
```python
def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": "Hello from Docker Lambda!"
    }
```

**requirements.txt**
```txt
# Add dependencies here (optional)
```

**Dockerfile**
```dockerfile
FROM public.ecr.aws/lambda/python:3.12 
WORKDIR ${LAMBDA_TASK_ROOT}
COPY requirements.txt .
RUN pip install --upgrade pip &&     pip install --no-cache-dir --prefer-binary -r requirements.txt
COPY main.py .
CMD ["main.lambda_handler"]
```

**serverless.yml**
```yaml
service: my-app-test

provider:
  name: aws
  runtime: provided.al2
  region: us-east-1
  timeout: 29
  memorySize: 2048
  ecr:
    images:
      app-lambda-test:
        path: .
        platform: linux/amd64
        provenance: false

functions:
  chatbot:
    image:
      name: app-lambda-test
    architecture: x86_64
    events:
      - http:
          path: chat
          method: post
    environment:
      GROQ_API_KEY: ${env:GROQ_API_KEY}
```

---

## 🐳 Docker Setup
Initialize Docker Buildx:
```bash
docker buildx create --use
docker buildx inspect --bootstrap
```

---

## 🚀 Deployment
Deploy the Lambda function:
```bash
serverless deploy
```

Remove all deployed resources:
```bash
serverless remove
```

---

## ✅ Summary
This project demonstrates **end-to-end serverless deployment** of a Python 3.12 Lambda function using **Docker containers** and the **Serverless Framework** on AWS, ensuring compatibility on Windows (x86_64).

---
