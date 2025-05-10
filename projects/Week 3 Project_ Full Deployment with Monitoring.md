Here is the detailed **Week 3 Project Report** along with the **code snippets**:

---

# ✅ **Week 3 Project Report**

## 📌 **Title:**

**Application Deployment with Azure Web App and Observability**

---

## 🎯 **Objective:**

Automate Continuous Deployment to Azure Web App with monitoring via **Application Insights**, **Log Analytics**, and **alerting rules**.

---

## 📝 **Project Description:**

This project extends the CI pipeline into a complete CI/CD pipeline:

* Deploys a Node.js app to Azure Web App using **Azure Pipelines (YAML)**.
* Sets up **Staging** and **Production deployment slots**.
* Implements **slot swapping**.
* Configures **Azure Monitor**, **Application Insights**, and **alert rules** for observability.

---

## 🔧 **Tools & Services Used:**

* Azure DevOps (Repos, Pipelines)
* Azure Web App (App Service)
* Application Insights
* Log Analytics Workspace
* Azure CLI / ARM Templates
* GitHub or Azure Repos

---

## 🗂️ **Directory Structure:**

```
project-root/
├── azure-pipelines.yml
├── app/                   # Node.js App
│   ├── server.js
│   └── ...
├── arm-templates/
│   ├── webapp.json
│   └── appinsights.json
└── README.md
```

---

## 📁 **Sample Code Files**

### `azure-pipelines.yml`

```yaml
trigger:
- main

variables:
  azureSubscription: 'Azure-Connection-Name'
  webAppName: 'mywebapp-demo'
  slotName: 'staging'

stages:
- stage: Build
  jobs:
  - job: Build
    pool:
      vmImage: ubuntu-latest
    steps:
    - task: NodeTool@0
      inputs:
        versionSpec: '16.x'
    - script: |
        npm install
        npm run build
      displayName: 'Install and Build'
    - task: PublishBuildArtifacts@1
      inputs:
        pathToPublish: '$(Build.ArtifactStagingDirectory)'
        artifactName: 'drop'

- stage: Deploy
  dependsOn: Build
  jobs:
  - deployment: DeployToStaging
    environment: 'staging'
    strategy:
      runOnce:
        deploy:
          steps:
          - download: current
            artifact: drop
          - task: AzureWebApp@1
            inputs:
              azureSubscription: $(azureSubscription)
              appName: $(webAppName)
              deployToSlotOrASE: true
              resourceGroupName: 'myResourceGroup'
              slotName: $(slotName)
              package: $(Pipeline.Workspace)/drop
  - deployment: SwapSlots
    dependsOn: DeployToStaging
    environment: 'production'
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureAppServiceManage@0
            inputs:
              azureSubscription: $(azureSubscription)
              action: 'Swap Slots'
              WebAppName: $(webAppName)
              ResourceGroupName: 'myResourceGroup'
              SourceSlot: $(slotName)
              TargetSlot: 'production'
```

---

### `webapp.json` (ARM Template for Web App with App Insights)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "resources": [
    {
      "type": "Microsoft.Web/sites",
      "apiVersion": "2021-02-01",
      "name": "[parameters('webAppName')]",
      "location": "[parameters('location')]",
      "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', parameters('appServicePlanName'))]"
      },
      "resources": [
        {
          "type": "slots",
          "apiVersion": "2021-02-01",
          "name": "staging",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites', parameters('webAppName'))]"
          ],
          "properties": {
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', parameters('appServicePlanName'))]"
          }
        }
      ]
    }
  ]
}
```

---

## 📊 **Monitoring Setup:**

1. **Application Insights:**

   * Linked to Web App from Azure Portal or using ARM.
   * Tracks exceptions, response times, live metrics.

2. **Log Analytics:**

   * Logs routed from App Insights for advanced queries.

3. **Alert Rules (Example):**

   * Alert if average response time > 3s for 5 mins.
   * Alert if availability drops below 99%.

---

## ✅ **Deliverables:**

* Working CI/CD pipeline with slot deployment & swapping
* Web App deployed on Azure with monitoring
* Application Insights dashboard
* Alerts visible in Azure Monitor

---

Would you like this structured as a downloadable report or need a sample Node.js app included too?
