Here is a complete **Week 4 Project Report** and the **code/scripts** for:
**✅ Week 4 Project: Azure Integration with DevOps Pipelines**

---

## 📄 **Project Report**

### 🎯 Title:

**End-to-End Integration with Azure Storage, Compute & Networking**

### 🎯 Objective:

Implement advanced Azure service integrations into a DevOps pipeline using Azure Pipelines with secure networking, artifact storage, and traffic routing.

---

### 🧾 Description:

In this project, you will:

* Use **Azure Blob Storage** for managing build artifacts.
* Deploy a **Web App** connected to a **Virtual Network (VNet)**.
* Configure **App Service Plan auto-scaling rules**.
* Secure WebApp access using **Access Policies** and **Private Endpoints**.
* Implement traffic management using **Azure Application Gateway** or **Azure Front Door**.
* Automate everything using **Azure DevOps YAML Pipelines**.

---

## 🛠️ **Project Architecture Components**

| Component              | Purpose                                 |
| ---------------------- | --------------------------------------- |
| Azure Blob Storage     | Store pipeline artifacts                |
| Azure Web App          | Host the deployed application           |
| Azure VNet             | Securely connect App Service to backend |
| App Gateway/Front Door | Route traffic with security policies    |
| Azure DevOps           | CI/CD automation                        |

---

## 🔧 Step-by-Step Implementation

### ✅ 1. **Provision Infrastructure using Azure CLI or Terraform**

Here's an example using **Azure CLI**:

```bash
# Create resource group
az group create --name DevOpsResourceGroup --location eastus

# Create storage account
az storage account create --name devopsstorage$RANDOM --resource-group DevOpsResourceGroup --location eastus --sku Standard_LRS

# Create blob container
az storage container create --account-name <storage-name> --name artifacts --public-access off

# Create App Service Plan
az appservice plan create --name DevOpsPlan --resource-group DevOpsResourceGroup --sku S1 --is-linux

# Create Web App
az webapp create --resource-group DevOpsResourceGroup --plan DevOpsPlan --name devopswebapp$RANDOM --runtime "NODE|18-lts"
```

---

### ✅ 2. **Sample Application Code** (Node.js)

```javascript
// app.js
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

app.get('/', (req, res) => res.send('🚀 Azure DevOps Integrated WebApp is live!'));

app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

**`package.json`**

```json
{
  "name": "azure-devops-app",
  "version": "1.0.0",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "test": "echo \"No tests yet\" && exit 0"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

---

### ✅ 3. **YAML Pipeline Code (azure-pipelines.yml)**

```yaml
trigger:
- main

variables:
  azureSubscription: 'AzureServiceConnection'
  appName: 'devopswebapp'
  resourceGroup: 'DevOpsResourceGroup'

stages:
- stage: Build
  jobs:
  - job: BuildJob
    pool:
      vmImage: 'ubuntu-latest'
    steps:
    - task: NodeTool@0
      inputs:
        versionSpec: '18.x'
    - script: |
        npm install
        npm run build || echo "No build step"
      displayName: 'Install dependencies and build'
    - task: ArchiveFiles@2
      inputs:
        rootFolderOrFile: '$(System.DefaultWorkingDirectory)'
        includeRootFolder: false
        archiveType: 'zip'
        archiveFile: '$(Build.ArtifactStagingDirectory)/app.zip'
        replaceExistingArchive: true
    - task: PublishBuildArtifacts@1
      inputs:
        PathtoPublish: '$(Build.ArtifactStagingDirectory)'
        ArtifactName: 'drop'

- stage: Deploy
  dependsOn: Build
  jobs:
  - deployment: DeployWebApp
    environment: 'production'
    strategy:
      runOnce:
        deploy:
          steps:
          - download: current
            artifact: drop
          - task: AzureWebApp@1
            inputs:
              azureSubscription: $(azureSubscription)
              appType: webAppLinux
              appName: $(appName)
              package: $(Pipeline.Workspace)/drop/app.zip
```

---

### ✅ 4. **App Gateway or Azure Front Door (Optional - Basic Setup)**

```bash
# Example: App Gateway Creation (simplified)
az network application-gateway create \
  --name DevOpsAppGateway \
  --resource-group DevOpsResourceGroup \
  --location eastus \
  --sku Standard_v2 \
  --capacity 2 \
  --vnet-name DevOpsVNet \
  --subnet AppGatewaySubnet
```

> 💡 *You can configure routing rules and attach a WAF policy for added security.*

---

## 📈 Additional Enhancements

* Add **Private Endpoint** to the App Service.
* Integrate **Application Insights** for monitoring.
* Set up **Auto-scaling rules**:

```bash
az monitor autoscale create \
  --resource-group DevOpsResourceGroup \
  --resource devopswebapp \
  --resource-type Microsoft.Web/sites \
  --name DevOpsAutoScale \
  --min-count 1 \
  --max-count 3 \
  --count 1
```

---

## 📦 Output

✅ Application deployed and accessible via public URL or App Gateway/Front Door
✅ Artifacts stored securely in Azure Blob
✅ Deployment logs and history stored in Azure DevOps
✅ Traffic management and scaling rules verified

---

Would you like a downloadable PDF version of this report, or to deploy this sample on your Azure subscription?
