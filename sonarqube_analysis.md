# sonarqube
### ✅ What is SonarQube?

**SonarQube** is an **open-source platform** used for **continuous inspection of code quality**. It **analyzes source code** to detect issues like:

* Bugs
* Code smells
* Security vulnerabilities
* Duplicated code
* Test coverage
* Technical debt

It supports **multiple programming languages** including Java, Python, JavaScript, C, C++, PHP, and more.

---

### 🎯 Why Use SonarQube?

| Reason                          | Description                                                                                |
| ------------------------------- | ------------------------------------------------------------------------------------------ |
| **1. Improve Code Quality**     | Automatically checks code for issues before it reaches production.                         |
| **2. Find Bugs Early**          | Detects bugs and potential runtime errors during development.                              |
| **3. Enforce Coding Standards** | Helps teams follow consistent coding practices using rules and quality gates.              |
| **4. Track Code Health**        | Gives metrics on maintainability, reliability, and security over time.                     |
| **5. Support CI/CD**            | Easily integrates with Jenkins, GitHub Actions, GitLab CI, etc., for automated code scans. |
| **6. Reduce Technical Debt**    | Identifies areas where code needs refactoring, helping to clean up over time.              |
| **7. Ensure Security**          | Highlights known security vulnerabilities (OWASP Top 10) in your code.                     |
| **8. Central Dashboard**        | Provides a visual overview of code health for all projects in one place.                   |

---

## ✅ What You’ll Install for sonarqube application

* Java 17 (required for SonarQube)
* PostgreSQL (SonarQube database)
* SonarQube (latest)
* Swap (for memory issues)

---

## 🚀 Step-by-Step Installation of sonarqube in ubuntu

### 🔹 Step 1: Update and Install Dependencies

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install wget unzip gnupg2 -y
```

---

### 🔹 Step 2: Install Java 17

```bash
sudo apt install openjdk-17-jdk -y
java -version
```

You should see `openjdk 17...` ✅

---

### 🔹 Step 3: Install PostgreSQL and Create Sonar DB

```bash
sudo apt install postgresql postgresql-contrib -y
sudo -u postgres psql
```

Inside PostgreSQL prompt:

```sql
CREATE USER sonar WITH ENCRYPTED PASSWORD 'sonar123';
CREATE DATABASE sonarqube OWNER sonar;
\q
```

---

### 🔹 Step 4: Download and Extract SonarQube

```bash
cd /opt
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.5.1.90531.zip
sudo apt install unzip -y
sudo unzip sonarqube-10.5.1.90531.zip
sudo mv sonarqube-10.5.1.90531 sonarqube
sudo adduser --system --no-create-home --group --disabled-login sonar
sudo chown -R sonar:sonar /opt/sonarqube
```

---

### 🔹 Step 5: Configure SonarQube Database

```bash
sudo nano /opt/sonarqube/conf/sonar.properties
```

Uncomment and edit these lines:

```ini
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar123
sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube
```

---

### 🔹 Step 6: Increase System Limits for Elasticsearch

Edit:

```bash
sudo nano /etc/sysctl.conf
```

Add:

```bash
vm.max_map_count=262144
```

Apply it:

```bash
sudo sysctl -w vm.max_map_count=262144
```

Edit limits:

```bash
sudo nano /etc/security/limits.conf
```

Add:

```
* soft nofile 65536
* hard nofile 65536
```

---

### 🔹 Step 7: Add Swap Memory (if < 2 GB RAM)

```bash
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
free -h
```

---

### 🔹 Step 8: Create Systemd Service

```bash
sudo nano /etc/systemd/system/sonarqube.service
```

Paste this:

```ini
[Unit]
Description=SonarQube service
After=network.target

[Service]
Type=forking
ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop
User=sonar
Group=sonar
Restart=on-failure
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

---

### 🔹 Step 9: Start SonarQube

```bash
sudo systemctl daemon-reload
sudo systemctl enable sonarqube
sudo systemctl start sonarqube
```

Check status:

```bash
sudo systemctl status sonarqube
```

---

### 🔹 Step 10: Access SonarQube

Open browser and go to:

```
http://<your-server-ip>:9000
```

Login:

* **Username**: `admin`
* **Password**: `admin`


---

## ✅ Project creation

1. Go to SonarQube → create project
2. Get your **Project Key** and **Token**




---

### 🆕 Create a New Project

1. Click on the **“Create Project”** button on the dashboard
   *(or go to Projects → Manage Projects → Create Project)*

2. Enter:

   * **Project Display Name** → e.g., `Snake Game`
   * **Project Key** → e.g., `snake-game`

     * (This is what you'll use in `sonar-scanner` → `-Dsonar.projectKey=snake-game`)

3. Click **Set Up**

---

### 🔐 Generate an Authentication Token

1. Choose **“Manually”** when it asks “How do you want to analyze your project?”
2. Give a name for your token (e.g., `snake-token`)
3. Click **“Generate”**

📌 **Copy this token immediately!**
You won’t be able to see it again!

Use this token in:

```bash
-Dsonar.login=your_token_here
```

---
## ✅ SonarScanner CLI Installation on AWS Linux



### 🔹 Step 1: Update and Install Dependencies

```bash
sudo yum update -y
sudo yum install unzip wget java-17-amazon-corretto -y
```

✅ Make sure Java 17 is available:

```bash
java -version
```

---

### 🔹 Step 2: Download SonarScanner CLI

```bash
cd /opt
sudo wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
sudo unzip sonar-scanner-cli-5.0.1.3006-linux.zip
sudo mv sonar-scanner-5.0.1.3006-linux sonar-scanner
```

---

### 🔹 Step 3: Set Up Environment Variables

Edit the system profile file:

```bash
sudo nano /etc/profile.d/sonarscanner.sh
```

Paste the following:

```bash
export SONAR_SCANNER_HOME=/opt/sonar-scanner
export PATH=$PATH:$SONAR_SCANNER_HOME/bin
```

Then apply:

```bash
source /etc/profile.d/sonarscanner.sh
```

Test:

```bash
sonar-scanner -v
```

✅ You should see output like:

```
SonarScanner 5.0.1.3006
```

---

### 🔹 Step 4: Configure Default Server URL (Optional)

Edit the scanner config:

```bash
sudo nano /opt/sonar-scanner/conf/sonar-scanner.properties
```

Add this at the bottom:

```ini
sonar.host.url=http://<your-sonarqube-server-ip>:9000
```

Replace with your real IP/domain.


---
1. Go to your project directory.

Then run:

```bash

sonar-scanner \
  -Dsonar.projectKey=sonarqube \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://<sonar_host_ip>:9000 \
  -Dsonar.login=<TOKEN>
```

---


