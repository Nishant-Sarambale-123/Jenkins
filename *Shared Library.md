Here’s a **simple and clear explanation** of **Jenkins Shared Libraries** — perfect for interviews and real-world use 👇

---

## 📘 **What is a Jenkins Shared Library?**

A **Jenkins Shared Library** is a **reusable set of Groovy scripts and pipeline code** that you can share across multiple Jenkins pipelines.

👉 Instead of writing the same pipeline steps in every job,
you write them **once** in a shared library and **reuse** them.

---

## 💡 **Why Use Shared Libraries?**

| Problem                                                 | Solution                                |
| ------------------------------------------------------- | --------------------------------------- |
| You have many Jenkins pipelines repeating the same code | Put the common code in a shared library |
| You want to standardize build/deploy stages             | Shared library                          |
| You need to update all pipelines easily                 | Change once in library → affects all    |

---

## 🧩 **Basic Concept**

A shared library is like a **Git repository** that contains reusable pipeline code.

You can call its functions in your Jenkinsfile like this:

```groovy
@Library('my-shared-lib') _
```

Then use the functions:

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                myBuildStep()
            }
        }
    }
}
```

Here, `myBuildStep()` comes from the shared library.

---

## 🏗️ **Shared Library Folder Structure**

Here’s what the **Git repo structure** looks like:

```
(my-shared-lib)
 ├── vars/
 │    └── myBuildStep.groovy
 │    └── deployApp.groovy
 ├── src/
 │    └── org/company/utils/Helper.groovy
 └── resources/
      └── templates/
```

### 📁 Folder Explanation:

| Folder         | Purpose                                                        |
| -------------- | -------------------------------------------------------------- |
| **vars/**      | Contains pipeline functions (easily callable from Jenkinsfile) |
| **src/**       | For advanced Groovy classes (like helper utilities)            |
| **resources/** | For external files (templates, configs, etc.)                  |

---

## ⚙️ **How to Configure in Jenkins**

### Step 1: Create Git Repo for Library

* Example repo name: `jenkins-shared-lib`
* Add the structure above and push it to GitHub/GitLab.

### Step 2: Add Library in Jenkins

1. Go to **Manage Jenkins → Configure System**
2. Scroll to **Global Pipeline Libraries**
3. Add:

   * **Name:** `my-shared-lib`
   * **Default version:** `main` (or `master`)
   * **Source Code Management:** Git
   * **Repository URL:** `https://github.com/org/jenkins-shared-lib.git`
4. Save ✅

### Step 3: Use in Jenkinsfile

```groovy
@Library('my-shared-lib') _

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                myBuildStep()
            }
        }
        stage('Deploy') {
            steps {
                deployApp()
            }
        }
    }
}
```

---

## 🧠 **Example: vars/myBuildStep.groovy**

```groovy
def call() {
    echo "Building the application..."
    sh 'mvn clean package'
}
```

## 🧠 **Example: vars/deployApp.groovy**

```groovy
def call() {
    echo "Deploying the application..."
    sh 'kubectl apply -f deployment.yaml'
}
```

---

## 🎯 **Advantages**

✅ Reusable — write once, use in many pipelines
✅ Easy maintenance — update one place
✅ Cleaner Jenkinsfiles — shorter, simpler code
✅ Standardization — all teams follow same process

---

## ⚡ **Pro Tip**

You can have:

* **Global Shared Library** → available to all pipelines (configured in Jenkins settings)
* **Local Shared Library** → used only in one repo (defined inside Jenkinsfile using Git URL)

Example:

```groovy
library identifier: 'my-lib@main', retriever: modernSCM([
  $class: 'GitSCMSource', remote: 'https://github.com/org/my-lib.git'
])
```

---

### ✅ **In Short**

| Term               | Meaning                                     |
| ------------------ | ------------------------------------------- |
| Shared Library     | Reusable pipeline code stored in Git        |
| `@Library('name')` | Imports the library                         |
| `vars/`            | Stores simple pipeline functions            |
| `src/`             | Stores complex helper classes               |
| Benefit            | Code reuse, cleaner pipelines, easy updates |

---

Would you like me to give a **real Jenkinsfile + shared library example** (for build, test, deploy stages) — the kind asked in interviews?
