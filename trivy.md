Below are **clear, interview-ready Trivy notes** followed by **step-by-step configuration instructions** (real-world + Jenkins focused).

---

# 🔐 Trivy – Complete Notes (DevOps / Interview Ready)

## ✅ What is Trivy?

**Trivy** is an **open-source vulnerability scanner** by Aqua Security used to detect security issues in:

* Docker images
* File systems
* Git repositories
* Kubernetes clusters
* SBOMs (Software Bill of Materials)

It is widely used in **CI/CD pipelines** to prevent vulnerable code or images from reaching production.

---

## 🔍 What Trivy Scans

| Scan Type     | What it Finds                                     |
| ------------- | ------------------------------------------------- |
| OS packages   | CVEs in OS libraries (Alpine, Ubuntu, RHEL, etc.) |
| Language deps | Java, Python, Node.js, Go vulnerabilities         |
| IaC           | Terraform, Kubernetes YAML misconfigs             |
| Secrets       | Hardcoded passwords, tokens                       |
| Licenses      | Open-source license issues                        |

---

## 🧠 Why Trivy is Used in CI/CD

* Shift-left security
* Fast (seconds)
* No external DB setup
* Simple CLI
* Easy Jenkins/GitHub/GitLab integration

---

## 📌 Common Trivy Scan Modes

### 1️⃣ Image Scan

```bash
trivy image nginx:latest
```

### 2️⃣ Filesystem Scan

```bash
trivy fs .
```

### 3️⃣ Repo Scan

```bash
trivy repo https://github.com/org/repo
```

### 4️⃣ Kubernetes Scan

```bash
trivy k8s --report summary cluster
```

---

## ⚙️ Important Trivy Options

| Option                     | Purpose                      |
| -------------------------- | ---------------------------- |
| `--severity HIGH,CRITICAL` | Scan only serious issues     |
| `--exit-code 1`            | Fail pipeline if vulns found |
| `--ignore-unfixed`         | Ignore unfixed CVEs          |
| `--no-progress`            | Clean CI logs                |
| `--format html/json`       | Generate reports             |

---

## ⭐ Interview One-Liner

> *“Trivy is an open-source vulnerability scanner integrated into CI/CD pipelines to scan container images, code, and IaC for security vulnerabilities before deployment.”*

---

# 🛠️ Trivy Configuration – Step by Step

## ✅ Step 1: Install Trivy (Jenkins Agent / Server)

### 🔹 Amazon Linux / RHEL

```bash
sudo yum install -y trivy
```

### 🔹 Ubuntu

```bash
sudo apt-get install -y trivy
```

### 🔹 Manual (Universal)

```bash
curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh
sudo mv trivy /usr/local/bin/
```

Verify:

```bash
trivy --version
```

---

## ✅ Step 2: Jenkins Agent Requirements

Make sure Jenkins agent has:

* Docker installed & running
* Trivy installed
* Network access to vulnerability DB

---

## ✅ Step 3: Add Trivy to Jenkins Pipeline

### 🔹 Typical Jenkins Stage

```groovy
stage('Trivy Image Scan') {
    steps {
        sh """
        trivy image \
          --severity HIGH,CRITICAL \
          --exit-code 1 \
          --no-progress \
          my-image:${BUILD_NUMBER}
        """
    }
}
```

✔ Pipeline **fails automatically** on vulnerabilities
✔ Blocks insecure images from production

---

## ✅ Step 4: Generate Trivy Report (Optional)

```bash
trivy image --format html --output trivy-report.html my-image:latest
```

In Jenkins:

```groovy
archiveArtifacts artifacts: 'trivy-report.html'
```

---

## ✅ Step 5: Ignore Known CVEs (Optional)

Create `.trivyignore`:

```text
CVE-2023-12345
CVE-2022-98765
```

Run:

```bash
trivy image --ignorefile .trivyignore my-image
```

---

## 🔒 Best Practices (Production)

✅ Scan **before pushing** image
✅ Fail build on **HIGH/CRITICAL**
✅ Use `.trivyignore` carefully
✅ Store reports as artifacts
✅ Combine with **Sonar + SAST**

---

## 🎯 Trivy vs Other Tools (Quick)

| Tool    | Type                      |
| ------- | ------------------------- |
| Trivy   | Container + IaC + Secrets |
| Clair   | Image only                |
| Snyk    | SaaS-based                |
| Anchore | Enterprise                |
| Aqua    | Enterprise                |

---

## 🧠 Final Interview Answer

> *“We use Trivy in Jenkins pipelines to scan Docker images for vulnerabilities before pushing to ECR. The pipeline fails automatically if HIGH or CRITICAL issues are found, ensuring secure deployments.”*

---

If you want next:

* Trivy **with Slack vulnerability summary**
* Trivy **filesystem + IaC scan**
* Trivy **Kubernetes admission controller**
* Trivy **SBOM generation**

Just tell me 👍
