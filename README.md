<div align="center">

# Azure DevOps Custom CI Pipeline (Time Boost)

### ⚡ Intelligent Build Optimization for Legacy .NET Solutions

[![GitHub stars](https://img.shields.io/github/stars/damianczer/azure-devops-msbuild-auto?style=for-the-badge&logo=github)](https://github.com/damianczer/azure-devops-msbuild-auto/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/damianczer/azure-devops-msbuild-auto?style=for-the-badge&logo=github)](https://github.com/damianczer/azure-devops-msbuild-auto/watchers)
[![GitHub issues](https://img.shields.io/github/issues/damianczer/azure-devops-msbuild-auto?style=for-the-badge&logo=github)](https://github.com/damianczer/azure-devops-msbuild-auto/issues)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

**Build only what changed, not the entire solution!**

[Features](#-key-features) •
[Problem](#-the-problem) •
[Solution](#-the-solution) •
[Setup](#-quick-start) •
[Documentation](#-documentation)

</div>

## 🎯 Overview

This PowerShell script **revolutionizes CI/CD pipelines** for large .NET solutions by implementing **intelligent selective building**. Instead of rebuilding hundreds of projects for a single file change, it analyzes Git diff and builds only affected projects.

Perfect for:
- 🏢 **Enterprise legacy applications** with 100+ projects
- 🔧 **Teams without Docker** or modern containerization
- ⏰ **Projects with 20+ minute build times**
- 💰 **Organizations with limited CI/CD budget**

## 😫 The Problem

### Does this sound familiar?

```
❌ Solution with 150+ projects
❌ Every PR triggers a full solution build
❌ 20-30 minutes waiting for a single line change
❌ Legacy codebase without Docker support
❌ No budget for proper CI/CD refactoring
```

**Traditional MSBuild approach:**
```xml
<MSBuild Projects="EntireSolution.sln" />
```
☝️ Rebuilds EVERYTHING, even if you changed one CSS file!

## ✅ The Solution

### Smart Selective Building

```mermaid
graph LR
    A[Feature Branch] -->|Git Diff| B[Changed Files]
    B --> C[Extract Projects]
    C --> D[Build Only Changed]
    D --> E[20 min saved! ⚡]
```

**Our approach:**
```xml
<MSBuild Projects="$(projects)" />
```
☝️ Builds only changed projects + their dependencies!

## 🌟 Key Features

| Feature | Description |
|---------|-------------|
| 🎯 **Selective Building** | Analyzes Git changes and builds only affected projects |
| 🔍 **Dependency Tracking** | Automatically includes referenced projects |
| 🌿 **Branch Comparison** | Compares feature branches against master/develop |
| 🔄 **PR Support** | Native Azure DevOps Pull Request integration |
| 📊 **Frontend Detection** | Separate tracking for frontend changes |
| ⚙️ **Customizable** | Adapt to your project structure |
| 📈 **Pipeline Variables** | Sets Azure DevOps variables for next steps |

## 🛠️ Technology Stack

<div align="center">

| Technology | Purpose | Documentation |
|------------|---------|---------------|
| ![PowerShell](https://img.shields.io/badge/PowerShell-5391FE?style=for-the-badge&logo=powershell&logoColor=white) | Script Engine | [Learn More](https://learn.microsoft.com/en-us/powershell/) |
| ![MSBuild](https://img.shields.io/badge/MSBuild-512BD4?style=for-the-badge&logo=.net&logoColor=white) | Build System | [Learn More](https://learn.microsoft.com/en-us/visualstudio/msbuild/?view=vs-2022) |
| ![Azure DevOps](https://img.shields.io/badge/Azure_DevOps-0078D7?style=for-the-badge&logo=azure-devops&logoColor=white) | CI/CD Platform | [Learn More](https://learn.microsoft.com/en-us/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops) |
| ![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white) | Version Control | [Learn More](https://git-scm.com/) |

</div>

## 📊 Performance Comparison

### Before vs After

<table>
<tr>
<td width="50%">

#### ❌ Before (Traditional Build)
![Before](https://github.com/user-attachments/assets/8f72e8f9-7fcf-4d71-9df4-f183edb814d9)

**Full solution build**
- ⏱️ **~25 minutes**
- 🔨 **150 projects rebuilt**
- 💸 **High Azure costs**

</td>
<td width="50%">

#### ✅ After (Selective Build)
![After](https://github.com/user-attachments/assets/5e4e6e01-9a2e-46bd-8d34-cfdec00700ec)

**Only changed projects**
- ⚡ **~5 minutes**
- 🎯 **3-5 projects rebuilt**
- 💰 **80% cost reduction**

</td>
</tr>
</table>

<div align="center">

### 🎉 **Result: ~20 minutes saved per build!**

</div>

## 🚀 Quick Start

### Prerequisites

```powershell
# Required
- Azure DevOps account
- Git repository
- MSBuild installed
- PowerShell 5.1+
```

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/damianczer/azure-devops-msbuild-auto.git
   ```

2. **Add script to your pipeline**
   ```yaml
   steps:
   - task: PowerShell@2
     inputs:
       filePath: 'script.ps1'
       arguments: >
         -CompareSourceBranch "master"
         -BranchName "$(Build.SourceBranch)"
         -Repository "$(Build.SourcesDirectory)"
         -TargetBranch "$(System.PullRequest.TargetBranch)"
   ```

3. **Configure MSBuild task**
   ```yaml
   - task: MSBuild@1
     condition: eq(variables['isBuildable'], 'True')
     inputs:
       solution: '$(projects)'
   ```

## ⚙️ Configuration Guide

### Step 1: PowerShell Task Configuration

![Step Configuration](https://github.com/user-attachments/assets/446de0d0-5a15-41ef-8f04-c93038bb91e4)

**Parameters:**
- `CompareSourceBranch`: Base branch (e.g., `master`, `develop`)
- `BranchName`: Current branch from pipeline variable
- `Repository`: Source directory path
- `TargetBranch`: PR target branch (for PR builds)

### Step 2: Pipeline Variables

![Pipeline Variables](https://github.com/user-attachments/assets/2106bb8b-8bea-4547-80a3-7f95292a5488)

**Output Variables:**
- `projects`: Space-separated list of `.csproj` files
- `hasFrontendChanged`: Boolean flag for frontend changes
- `isBuildable`: Boolean flag if build is needed

### Step 3: MSBuild Configuration

![MSBuild Configuration](https://github.com/user-attachments/assets/b4f653cf-312c-47dd-8bce-5c26afea8ac4)

**Build.proj Example:**
```xml
<Project>
  <Target Name="BuildChanged">
    <MSBuild Projects="$(projects)" 
             Properties="Configuration=Release;Platform=Any CPU" 
             BuildInParallel="true" />
  </Target>
</Project>
```

## 🔍 How It Works

```powershell
# 1️⃣ Compare branches
git diff --name-only feature-branch..master

# 2️⃣ Extract changed files
src/Feature/Authentication/code/Models/User.cs
src/Feature/Catalog/code/Services/ProductService.cs
src/Frontend/App/components/Header.tsx

# 3️⃣ Find related .csproj files
src/Feature/Authentication/Authentication.csproj
src/Feature/Catalog/Catalog.csproj

# 4️⃣ Set pipeline variables
projects = "Auth.csproj Catalog.csproj"
hasFrontendChanged = true
isBuildable = true

# 5️⃣ MSBuild uses variables
<MSBuild Projects="$(projects)" />
```

## 🎨 Customization

### Adapt to Your Project Structure

The script looks for `/code` directory by default. Modify for your structure:

```powershell
# Current structure: src/Feature/FeatureName/code/
if ($element.Contains("/code")) {
    # Extract project path
}

# Your structure: src/Modules/ModuleName/
if ($element.Contains("/Modules/")) {
    $index = $element.IndexOf("/Modules/")
    # Custom logic
}
```

### Add Custom Patterns

```powershell
# Track database changes
if ($element.Contains("/Database/")) {
    $hasDatabaseChanged = $true
}

# Track API changes
if ($element.Contains("/API/")) {
    $hasAPIChanged = $true
}
```

## 📚 Documentation

### Script Parameters

| Parameter | Required | Description | Example |
|-----------|----------|-------------|---------|
| `CompareSourceBranch` | ✅ | Branch to compare against | `master`, `develop` |
| `BranchName` | ✅ | Current branch name | `feature/new-feature` |
| `Repository` | ✅ | Repository path | `C:\Repos\MyProject` |
| `TargetBranch` | ❌ | PR target branch | `refs/heads/develop` |

### Output Variables

```yaml
variables:
  projects: 'Project1.csproj Project2.csproj'
  hasFrontendChanged: 'true'
  isBuildable: 'true'
```

## 🤝 Contributing

Contributions are welcome! Here's how:

1. 🍴 Fork the repository
2. 🌿 Create feature branch (`git checkout -b feature/AmazingFeature`)
3. 💾 Commit changes (`git commit -m 'Add AmazingFeature'`)
4. 📤 Push to branch (`git push origin feature/AmazingFeature`)
5. 🔀 Open Pull Request

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

```
MIT License - Copyright (c) 2025 Damian Czerwiński
```

## 👨‍💻 Author

<div align="center">

### Damian Czerwiński

**If this project helped you, please ⭐ star this repository!**

</div>

<div align="center">

### 💡 Questions or Issues?

[Open an Issue](https://github.com/damianczer/azure-devops-msbuild-auto/issues) • [Discussions](https://github.com/damianczer/azure-devops-msbuild-auto/discussions)

**Made with ❤️ for the .NET Community**

</div>
