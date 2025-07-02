## 📦 Create a New Agent Node in Jenkins**

**Steps:**

1. Go to Jenkins Dashboard → **Manage Jenkins** → **Manage Nodes and Clouds** → **New Node**

2. Provide the following:

   * **Node Name**: `testnode`
   * **Type**: Permanent Agent
![image 1](<image/2js.png>)

3. Click `Create`.
![image 1](<image/1js.png>)
---



---

### 📝 Configure the Agent Node**

Fill in the following details:

* **Number of Executors**: `1`
* **Remote root directory**: `/opt/`
* **Labels**: `test`
* **Launch method**: *Launch agent by connecting it to the controller*
* **Availability**: *Keep this agent online as much as possible*

Then click **Save**.
![image 1](<image/3js.png>)

---

### 🔗 **4. Get the Agent Connection Command**

1. Click on the new node (`testnode`) → scroll to **“Run from agent command line (Unix)”**
2. Copy the command shown. It looks like:

```bash
curl -sO http://<controller-ip>:8080/jnlpJars/agent.jar
java -jar agent.jar -url http://<controller-ip>:8080/ -secret <secret-key> -name testnode -webSocket -workDir "/opt/"
```

![image 1](<image/4js.png>)
![image 1](<image/5js.png>)

---

### 💻 **5. Run Agent on Linux/EC2 Machine**

On your agent machine (e.g., EC2 instance):

1. Switch to `/opt/` directory:

   ```bash
   cd /opt/
   ```

2. Paste and run the copied command:

   ```bash
   curl -sO http://13.235.86.201:8080/jnlpJars/agent.jar
   nohup java -jar agent.jar -url http://13.235.86.201:8080/ -secret <secret-key> -name testnode -webSocket -workDir "/opt/" &
   ```

> `nohup` runs it in background and keeps it alive after logout.

![image 1](<image/6js.png>)
---

### ✅ **6. Verify Agent is Connected**

* Go back to Jenkins → **Nodes**
* You’ll see `testnode` listed and status **"Agent is connected"** if successful.
  ![Node List](file-WnHpSw5NtqqrBxPs3pvN1e)
  ![Agent Connected](file-EUCxoZhFiu5Q2dfYw4BCsk)
  ![Testnode Dashboard](file-VbXizNd94JYDShxavAjYBC)
![image 1](<image/7js.png>)
---

