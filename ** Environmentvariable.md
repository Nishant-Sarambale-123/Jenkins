In **Jenkins**, environment variables can be defined in multiple places depending on the scope (global, pipeline, stage, or job level). These are common places used in real DevOps pipelines.

---

## 1. Global Environment Variables (Jenkins System Level)

Defined for **all jobs in Jenkins**.

**Path:**
`Manage Jenkins → Configure System → Global properties → Environment variables`

**Example:**

```
Name: APP_ENV
Value: production
```

**Use case:**
Common variables like region, environment name, or API URL.

---

## 2. Jenkins Job Configuration

You can define environment variables **inside a specific job**.

**Path:**
`Job → Configure → Build Environment → Inject environment variables`

Example:

```
DB_HOST=database.example.com
APP_ENV=dev
```

Scope: Only that job.

---

## 3. Inside Jenkins Pipeline (Jenkinsfile)

You can define environment variables directly in the pipeline code.

Example:

```groovy
pipeline {
    agent any

    environment {
        APP_ENV = "production"
        VERSION = "1.0"
    }

    stages {
        stage('Build') {
            steps {
                echo "Environment is ${APP_ENV}"
            }
        }
    }
}
```

---

## 4. Stage-Level Environment Variables

Defined for a **specific stage** only.

```groovy
stage('Test') {
    environment {
        TEST_ENV = "qa"
    }
    steps {
        echo "Testing in ${TEST_ENV}"
    }
}
```

Scope: Only that stage.

---

## 5. Using `withEnv` Block

Temporary environment variables inside pipeline steps.

```groovy
withEnv(["APP_ENV=dev"]) {
    sh 'echo $APP_ENV'
}
```

Scope: Only inside this block.

---

## 6. Using Credentials as Environment Variables

Sensitive values like passwords or tokens.

Example:

```groovy
environment {
    DB_PASSWORD = credentials('db-pass-id')
}
```

Stored securely in **Jenkins Credentials Store**.

---

## 7. From Shell Script

You can export variables in shell commands.

```groovy
sh '''
export APP_ENV=dev
echo $APP_ENV
'''
```

But this variable only exists in that shell session.

---

## Summary Table

| Location                     | Scope                     |
| ---------------------------- | ------------------------- |
| Global properties            | All Jenkins jobs          |
| Job configuration            | Single job                |
| Pipeline `environment` block | Entire pipeline           |
| Stage environment            | Only that stage           |
| `withEnv`                    | Temporary block           |
| Credentials                  | Secure variables          |
| Shell export                 | Only that shell execution |

---

✅ **Interview Tip:**
If an interviewer asks *“Where can you define environment variables in Jenkins?”*
You can answer:

> In Jenkins, environment variables can be defined at multiple levels such as **global configuration, job configuration, Jenkinsfile pipeline environment block, stage-level environment, withEnv block, credentials store, and shell scripts.**

---

If you want, I can also show **how to store environment variables outside the Jenkinsfile (best practice for DevOps interviews)**.
