# jenkins master setup in ubuntu

---

### ✅ Step 1: Update System

```bash
sudo apt update
sudo apt upgrade -y
```

---

### ✅ Step 2: Install Java (Jenkins requires Java 11 or 17)

```bash
sudo apt install openjdk-17-jdk -y
```

👉 Confirm Java installation:

```bash
java -version
```

---

### ✅ Step 3: Add Jenkins Repository

```bash
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

```bash
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

---

### ✅ Step 4: Install Jenkins

```bash
sudo apt update
sudo apt install jenkins -y
```

---

### ✅ Step 5: Start and Enable Jenkins

```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

👉 Check status:

```bash
sudo systemctl status jenkins
```

---


### ✅ Step 7: Access Jenkins Web Interface

Open browser and go to:

```
http://your-server-ip:8080
```

---

### ✅ Step 8: Get Initial Admin Password

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy the password and paste it into the Jenkins setup screen in your browser.

---

### ✅ Step 9: Install Suggested Plugins

* After login, Jenkins will ask you to **install suggested plugins** – click it.
* Create your **first admin user**.
* Jenkins is ready!

---


