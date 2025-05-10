Here’s the full **project report** and **sample YAML pipeline code** for:

---

### ✅ **Week 2 Project: CI Pipeline for Node.js App**

---

### 🧾 **Project Title:**

**CI/CD Pipeline Development with YAML for a Node.js Web App**

---

### 🎯 **Objective:**

Automate the build and test process for a Node.js application using Azure Pipelines configured with YAML. Ensure code quality and CI best practices.

---

### 📝 **Description:**

In this project, we use a sample Node.js application and develop a CI pipeline that:

* Installs all necessary Node.js dependencies
* Runs unit tests
* Publishes build artifacts for deployment

The YAML-based Azure Pipeline utilizes:

* **Pipeline variables** for dynamic configuration
* **Variable groups** for storing secrets and shared values
* **Templates** for modular and reusable YAML code

---

### 📁 **Directory Structure:**

```
nodejs-app/
├── azure-pipelines.yml
├── app.js
├── package.json
├── tests/
│   └── app.test.js
```

---

### 📄 **Sample Code Files**

#### `app.js`

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello from Node.js App via Azure DevOps CI!');
});

module.exports = app;
```

---

#### `package.json`

```json
{
  "name": "azure-node-ci",
  "version": "1.0.0",
  "scripts": {
    "test": "jest",
    "start": "node app.js"
  },
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "jest": "^29.0.0",
    "supertest": "^6.1.3"
  }
}
```

---

#### `tests/app.test.js`

```javascript
const request = require('supertest');
const app = require('../app');

describe('GET /', () => {
  it('should return Hello message', async () => {
    const res = await request(app).get('/');
    expect(res.statusCode).toEqual(200);
    expect(res.text).toContain('Hello');
  });
});
```

---

### ⚙️ **Azure Pipeline YAML: `azure-pipelines.yml`**

```yaml
trigger:
  branches:
    include:
      - main

variables:
  buildConfiguration: 'Release'

stages:
- stage: BuildAndTest
  displayName: 'Build and Test Stage'
  jobs:
    - job: Build
      displayName: 'Install Dependencies and Run Tests'
      pool:
        vmImage: 'ubuntu-latest'

      steps:
        - task: NodeTool@0
          inputs:
            versionSpec: '18.x'
          displayName: 'Use Node.js'

        - script: |
            npm install
          displayName: 'Install Dependencies'

        - script: |
            npm test
          displayName: 'Run Unit Tests'

        - task: PublishBuildArtifacts@1
          inputs:
            PathtoPublish: '$(System.DefaultWorkingDirectory)'
            ArtifactName: 'drop'
            publishLocation: 'Container'
```

---

### 🔐 **Optional: Link Variable Groups**

To use secrets (like connection strings, tokens, etc.):

1. Go to Azure DevOps → Pipelines → Library
2. Create a Variable Group named `NodeSecrets`
3. Add secrets (e.g., `NPM_TOKEN`, `ENV`) and mark as secret
4. Link in the pipeline YAML:

```yaml
variables:
- group: NodeSecrets
```

---

### 📦 **Expected Output**

* Node.js dependencies installed successfully
* All tests pass
* Artifacts are generated and uploaded to the pipeline for CD usage

---

Would you like me to add a **CD stage for Azure Web App deployment** as well?
