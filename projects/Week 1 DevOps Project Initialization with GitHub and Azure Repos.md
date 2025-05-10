Here is a complete **Project Report with Code** for **✅ Week 1 Project: Git-Integrated DevOps Onboarding**.

---

# 📘 **Project Report: DevOps Project Initialization with GitHub and Azure Repos**

## ✅ Week 1 Project

---

## 📌 **Title**

**DevOps Project Initialization with GitHub and Azure Repos**

---

## 🎯 **Objective**

To help learners:

* Set up and navigate an Azure DevOps project
* Integrate GitHub/Bitbucket repositories with Azure DevOps
* Apply Git branching strategies (feature branches, PRs, merges)
* Enforce repository policies for cleaner version control

---

## 📝 **Description**

This project simulates a real-world DevOps onboarding task. Learners will:

* Create an Azure DevOps organization and project.
* Integrate it with a GitHub or Bitbucket repository.
* Apply Git best practices: feature branching, pull requests, and merges.
* Configure branch policies such as required reviews, check policies, and CI triggers.

This ensures team collaboration, commit hygiene, and version control standards are maintained in development environments.

---

## 🔧 **Tools & Technologies**

* Azure DevOps
* GitHub / Bitbucket
* Git CLI
* Azure Repos
* Visual Studio Code (optional)

---

## 📂 **Project Steps**

### Step 1: Create Azure DevOps Project

1. Login to [Azure DevOps](https://dev.azure.com)
2. Create a new organization if not already done.
3. Click **"New Project"**

   * Name: `devops-onboarding-project`
   * Visibility: Private
   * Version control: Git
   * Work item process: Basic

### Step 2: Create or Use a GitHub Repository

```bash
# Create a local directory
mkdir devops-onboarding-project
cd devops-onboarding-project

# Initialize Git and push to GitHub
git init
echo "# DevOps Project Onboarding" >> README.md
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/<your-username>/devops-onboarding-project.git
git push -u origin master
```

### Step 3: Link GitHub Repo to Azure DevOps

1. Go to Azure DevOps → Repos
2. Choose “Import Repository”
3. Select GitHub (OAuth authorization may be required)
4. Select your repo and import

**OR**

1. Go to **Project Settings > Service Connections**
2. Create a new connection to GitHub (OAuth)
3. Use the pipeline or repo import tools with this connection

### Step 4: Set Branching Policy in Azure DevOps

1. Navigate to **Repos > Branches**
2. Click on the `main` branch → **Branch Policies**

   * ✔ Require a minimum of 1 reviewer
   * ✔ Check for linked work items
   * ✔ Enforce merge strategy
   * ✔ Build validation (optional for advanced projects)

---

## 🚀 **Practice Git Flow**

### Create Feature Branch

```bash
git checkout -b feature/login
touch login.html
git add login.html
git commit -m "Add login page"
git push origin feature/login
```

### Raise Pull Request (PR)

1. Go to **Repos > Pull Requests**
2. Create PR: `feature/login` → `main`
3. Assign reviewers, add comments, and complete PR after approval

### Merge Pull Request

* Review code changes
* Approve PR
* Click “Complete” to merge (with squash or rebase merge strategies)

---

## 💻 **Sample Directory Structure**

```
devops-onboarding-project/
├── README.md
├── .gitignore
├── feature/
│   └── login.html
└── main/
    └── index.html
```

---

## 🧪 **Verification Checklist**

✅ Azure DevOps project created
✅ GitHub repo created and connected
✅ Feature branch created and PR raised
✅ Branch policies enforced
✅ PR approved and merged

---

## 📸 **Screenshots (Recommended in Report)**

* Azure DevOps project dashboard
* GitHub repository
* Feature branch in Azure Repos
* Branch policy settings
* Pull Request screenshot

---

## 🧠 **Learning Outcomes**

* Understood Azure DevOps project and Git integration
* Practiced Git branching and pull request workflow
* Enforced version control best practices
* Promoted collaborative DevOps culture

---

Would you like a downloadable **Word or PDF version** of this project report with placeholders for screenshots and evaluation checklist?
