Here are the **simple, step-by-step instructions** to configure **Slack with Jenkins** 👇

---

### 🧩 **Objective:**

Send Jenkins build notifications (success/failure) directly to a Slack channel.

---

### ⚙️ **Steps to Configure Slack with Jenkins:**

#### **Step 1: Install Slack Plugin in Jenkins**

1. Go to **Jenkins Dashboard → Manage Jenkins → Manage Plugins**
2. Click on the **Available** tab
3. Search for **"Slack Notification"**
4. Check it → Click **Install without restart**

---

#### **Step 2: Create a Slack App / Token**

1. Go to [https://api.slack.com/apps](https://api.slack.com/apps)
2. Click **Create New App** → choose **From scratch**
3. Give it a name (e.g., `JenkinsNotifier`) and select your Slack workspace
4. In the left menu → go to **OAuth & Permissions**
5. Under **Bot Token Scopes**, add:

   * `chat:write`
   * `chat:write.public`
   * `channels:read`
6. Click **Install App to Workspace** → **Allow**
7. Copy the **Bot User OAuth Token** (starts with `xoxb-...`)

---

#### **Step 3: Configure Slack in Jenkins**

1. Go to **Manage Jenkins → Configure System**
2. Scroll down to **Slack** section
3. Enter:

   * **Workspace / Team Domain:** (e.g., `yourteam.slack.com`)
   * **Integration Token Credential ID:** Click **Add → Jenkins** → choose **Secret Text**, and paste the `xoxb-` token
4. Test connection → click **Test Connection** (you’ll see a message in Slack if successful)

---

#### **Step 4: Configure Slack in a Jenkins Job**

1. Open your **Jenkins Job** → **Configure**
2. Go to **Post-build Actions** → click **Add post-build action**
3. Choose **Slack Notifications**
4. Select what you want to notify:

   * ✅ Success
   * ❌ Failure
   * ⚠️ Unstable
5. Save the job.

---

#### **Step 5: Test It**

Run a build → you’ll get a notification in your Slack channel showing build status and job link 🎉

---

### 🔧 Example Message You’ll See in Slack:

```
Jenkins Build Notification:
Job: my-pipeline
Status: SUCCESS ✅
URL: http://jenkins-server/job/my-pipeline/123/
```

---

Would you like me to also show **how to integrate Slack notifications inside a Jenkins pipeline script (`Jenkinsfile`)**?
