Here is the complete **Project Report** and **Code Structure** for **✅ Week 5 Capstone Project: Full-Stack CI/CD Pipeline with GitHub, Azure WebApp, Function App & Monitoring**.

---

## 📝 **Project Report**

### 🎯 Title

**Full-Stack CI/CD Pipeline with GitHub, Azure WebApp, Function App & Monitoring**

---

### 🎯 Objective

Apply complete Azure DevOps lifecycle using a real-world containerized full-stack application. Automate builds, deployments, background task execution, blob storage integration, and observability.

---

### 🧱 Architecture Components

* **Frontend:** React.js (hosted on Azure Web App)
* **Backend:** Node.js or Python Flask API (hosted on Azure Web App or Container App)
* **Background Jobs:** Azure Function App
* **File Storage:** Azure Blob Storage
* **CI/CD Tools:** GitHub Actions & Azure Pipelines
* **Monitoring:** Azure Application Insights with alerts

---

### 🔁 Workflow

1. **Code pushed to GitHub**
2. **GitHub Actions triggers CI workflow**
3. **CI builds Docker images & pushes to ACR**
4. **Azure Pipelines triggers CD**
5. **CD deploys to dev → staging → production**
6. **Function App is deployed separately**
7. **Blob Storage used by app for media/doc uploads**
8. **Application Insights monitors health**
9. **Alert Rules notify on failures or latency issues**

---

### 📁 Folder Structure

```
fullstack-ci-cd-devops/
├── .github/
│   └── workflows/
│       └── ci.yml
├── azure-pipelines/
│   ├── cd-dev.yml
│   ├── cd-staging.yml
│   └── cd-prod.yml
├── frontend/
│   ├── Dockerfile
│   └── src/
├── backend/
│   ├── Dockerfile
│   └── app/
├── function-app/
│   ├── function-code/
│   └── host.json
├── infrastructure/
│   ├── blob-storage.bicep
│   ├── app-insights.bicep
│   └── functionapp.bicep
├── README.md
```

---

## 🧪 **CI Pipeline (GitHub Actions)** – `.github/workflows/ci.yml`

```yaml
name: CI - Build and Push to ACR

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      ACR_NAME: myacrname

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Azure Login
        uses: azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}

      - name: Build and Push Frontend
        run: |
          az acr build --registry $ACR_NAME --image frontend:latest ./frontend

      - name: Build and Push Backend
        run: |
          az acr build --registry $ACR_NAME --image backend:latest ./backend
```

---

## 🚀 **CD Pipelines (Azure Pipelines)** – Sample `cd-dev.yml`

```yaml
trigger: none

stages:
  - stage: DeployDev
    jobs:
      - job: DeployWebApp
        pool:
          vmImage: 'ubuntu-latest'
        steps:
          - task: AzureWebAppContainer@1
            inputs:
              azureSubscription: 'AzureConnection'
              appName: 'fullstack-dev-app'
              containers: 'myacrname.azurecr.io/frontend:latest,myacrname.azurecr.io/backend:latest'
```

> ✅ Repeat similar pipelines for `cd-staging.yml` and `cd-prod.yml` with appropriate app names.

---

## ⚙️ **Azure Function App (Python Example)** – `function-app/function-code/HttpTrigger/__init__.py`

```python
import logging
import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Function triggered')
    return func.HttpResponse("Function executed successfully", status_code=200)
```

---

## 📊 **Monitoring Setup**

* Application Insights connected via ARM/Bicep during deployment
* Alerts for:

  * HTTP 5xx errors
  * Response time > 2s
  * CPU usage > 80%

---

## 💡 Tips

* Use GitHub Secrets and Azure Key Vault for sensitive values
* Define different app settings for each environment (dev/staging/prod)
* Set up custom domains & HTTPS on production Web App
* Use Blob lifecycle rules for cost optimization

---

Would you like a downloadable ZIP or GitHub-ready version of this project folder structure?
