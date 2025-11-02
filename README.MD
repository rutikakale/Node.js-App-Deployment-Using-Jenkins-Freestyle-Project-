## 🚀 Node.js Application Deployment using Jenkins (Freestyle CI/CD Pipeline)

This project demonstrates a Continuous Integration/Continuous Deployment (CI/CD) setup for a Node.js application using Jenkins Freestyle Jobs.
The pipeline runs in four sequential stages to automatically pull, build, and deploy the application.



-----

### 🧩 Pipeline Overview



| Job Name                  | Description                                         |
| ------------------------- | --------------------------------------------------- |
| **01-setup-env**          | Installs essential dependencies (Node.js, npm, PM2) |
| **02-node-pull**          | Pulls the latest code from GitHub                   |
| **03-install-dependency** | Installs Node.js project dependencies               |
| **04-node-deploy**        | Starts or restarts the app using PM2                |

-----
#### ✅ Once all stages complete successfully, the app becomes available at:
👉 http://3.85.10.55:3000/

-----
![](/IMG/Node.js%20Deployment%20on%20Jenkins.jpg)

-----
### 🛠️ Prerequisites

* A running **Jenkins** instance

* **Git** installed on the Jenkins server

* Jenkins user with permission to execute system commands

* **Note**: The first job (`01-setup-env`) uses `sudo` to install tools globally.
Later, `npm install -g pm2` ensures Jenkins owns the PM2 tool for deployment.

### ⚙️ Jenkins Job Configuration

#### 1️⃣ `01-setup-env` (Setup Environment)

**Objective**: Install Node.js, npm, and PM2 globally.

**Build Step → Execute Shell:**
```bash
sudo apt update
sudo apt install nodejs -y
sudo apt install npm -y
sudo npm install -g pm2
```
* **SCREENSHOT :**
![](/IMG/Screenshot%20(92).png)

-----

### 2️⃣ `02-node-pull` (Pull Repository)

**Objective:** Pull the latest application code.

**Source Code Management → Git**
**Repository URL**: https://github.com/iamtruptimane/node-js-app-CICD.git

**Branch**: `main`
* **SCREENSHOT :**

  ![](/IMG/Screenshot%20(94).png)

-----

###  3️⃣ `03-install-dependency` (Install Dependencies)
**Objective:** Install required Node.js packages.

**Trigger:** Build after 02-node-pull

**Build Step → Execute Shell:**
```bash
cd /var/lib/jenkins/workspace/02-node-pull/
sudo npm install
```
**Note:** The workspace name may differ. Adjust the path accordingly.

* **SCREENSHOT :**
![](/IMG/Screenshot%20(95).png)

-----

### 4️⃣ `04-node-deploy` (Deploy Application)

**Objective:** Deploy or restart the app using PM2.

**Trigger:** Build after 03-install-dependency

**Build Step → Execute Shell:**
```bash
cd /var/lib/jenkins/workspace/02-node-pull/
pm2 start app.js --name node-app || pm2 restart node-app
```
This command tries to start the app; if it’s already running, it restarts it instead.

* **SCREENSHOT :**
![](/IMG/Screenshot%20(96).png)

-----

### ✅ Deployment Status

* Jenkins console output confirms successful deployment.

* PM2 manages the Node.js process in fork mode.

* The app runs successfully on port 3000.

#### 🌐 **Access App:** http://3.85.10.55:3000/

(Default port `3000` is defined in the `Node.js` app configuration.)

**✅ Jenkins Console Output — Successful Build**

![](/IMG/Screenshot%20(93).png)

----