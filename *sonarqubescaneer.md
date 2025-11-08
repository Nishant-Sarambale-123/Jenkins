Here’s a **simple, step-by-step guide** to integrate **SonarQube Scanner with Jenkins** 👇

---

## 🎯 **Goal**

Automatically analyze code quality and security issues in Jenkins builds using **SonarQube**.

---

## ⚙️ **Step-by-Step Setup**

---

### 🧩 **Step 1: Install SonarQube and SonarScanner**

**Option 1:** Use an existing SonarQube server
**Option 2:** Install locally using Docker:

```bash
docker run -d --name sonarqube -p 9000:9000 sonarqube:lts
```

👉 Access it at: [http://localhost:9000](http://localhost:9000)
(Default login: `admin` / `admin`)

---

### 🧩 **Step 2: Install SonarQube Plugin in Jenkins**

1. Go to **Jenkins Dashboard → Manage Jenkins → Manage Plugins**
2. Click on **Available** tab
3. Search for **"SonarQube Scanner"**
4. Install it → Restart Jenkins if required

---

### 🧩 **Step 3: Configure SonarQube Server in Jenkins**

1. Go to **Manage Jenkins → Configure System**
2. Scroll to **SonarQube Servers**
3. Click **Add SonarQube**
4. Enter:

   * **Name:** `SonarQube`
   * **Server URL:** `http://<your-sonarqube-server>:9000`
   * **Authentication Token:**

     * In SonarQube → Go to **My Account → Security → Generate Token**
     * Copy it → In Jenkins, click **Add → Secret Text → Paste token**
5. Save the configuration ✅

---

### 🧩 **Step 4: Configure Sonar Scanner in Jenkins**

1. Go to **Manage Jenkins → Global Tool Configuration**
2. Scroll to **SonarQube Scanner**
3. Click **Add SonarQube Scanner**
4. Give a name (e.g., `SonarScanner`)
5. Select **Install automatically** or provide manual path if already installed
6. Save ✅

---

### 🧩 **Step 5: Add SonarQube Step in Jenkins Job**

**Option 1: Freestyle Project**

1. Create or open a **Jenkins Freestyle Project**
2. Go to **Build Environment**

   * Check **Prepare SonarQube Scanner environment**
3. In **Build Step → Add Build Step → Execute SonarQube Scanner**
4. Add analysis properties:

   ```bash
   sonar.projectKey=my-project
   sonar.projectName=My Project
   sonar.projectVersion=1.0
   sonar.sources=src
   sonar.language=java
   sonar.java.binaries=target/classes
   ```

---

**Option 2: Pipeline (Jenkinsfile)**

```groovy
pipeline {
    agent any
    tools {
        sonarQubeScanner 'SonarScanner'
    }
    stages {
        stage('Code Checkout') {
            steps {
                git 'https://github.com/example/my-repo.git'
            }
        }
        stage('SonarQube Analysis') {
            environment {
                SONARQUBE = credentials('sonar-token-id')
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner -Dsonar.projectKey=my-project -Dsonar.sources=src -Dsonar.java.binaries=target/classes'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
```

✅ This pipeline:

* Runs analysis
* Waits for SonarQube quality gate result
* Fails build if quality gate fails

---

### 🧩 **Step 6: Verify Results**

* Run the Jenkins job
* Open SonarQube dashboard → check your project appears with:

  * Code coverage
  * Bugs
  * Vulnerabilities
  * Code smells

---

### 🧠 **Bonus Tips**

* **Integration with GitHub/GitLab:** Automatically comments on PRs with issues
* **Quality Gates:** Block builds that fail minimum code standards
* **SonarCloud:** For cloud-based SonarQube (no server setup)

---

Would you like me to show how to configure **SonarQube + Jenkins + GitHub** together (for PR analysis and automatic quality gate checks)?
