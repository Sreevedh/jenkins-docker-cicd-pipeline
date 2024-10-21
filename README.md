# Jenkins-Docker CI/CD Pipeline

This project demonstrates how to set up a CI/CD pipeline using Jenkins and Docker across multiple virtual machines (VMs). The pipeline automates the process of building, pushing, and deploying Docker images.

---

## **Step 1: VM Setup**

1. **Create four VMs**:
   - VMs: **docker**, **jenkins**, **jenkins_slave**, and **docker_repo** (using Vagrant with CentOS OS).

2. **Install necessary components**:
   - Install **Jenkins** and **Docker** on both `jenkins` and `jenkins_slave` VMs.
   - Install **Docker** on `docker` and `docker_repo` VMs.
   - Install **Git** on both `jenkins` and `jenkins_slave` VMs.

3. **Update all VMs**:
   - Run the following command on all VMs:
     ```bash
     sudo yum update -y
     ```

4. **Access Jenkins UI**:
   - Open Jenkins in your browser at `localhost:8080`, install required plugins, and sign in.

---

## **Step 2: Setting up Jenkins Slave**

1. **Create or use an existing user**:
   - Install Jenkins on the `jenkins_slave` VM.

2. **Generate SSH keys on the `jenkins_slave` VM**:
   - Run the following command:
     ```bash
     ssh-keygen -t rsa
     ```
   - Copy the **private key** (`~/.ssh/id_rsa`) and paste it into the "New Node" configuration in Jenkins under the **Launch method** section for the new node.

3. **Configure the node in Jenkins**:
   - Go to **Manage Jenkins** → **Manage Nodes and Clouds** → **New Node** → Enter the node details for `jenkins_slave`.
   - Set **Host Key Verification Strategy** to **None** under the node's SSH configuration.

4. **Configure SSH on the `jenkins_slave` VM**:
   - Copy the **public key** (from the SSH key generated earlier):
     ```bash
     cat ~/.ssh/id_rsa.pub
     ```
   - Paste this public key into the `authorized_keys` file located in the `.ssh` directory of the `jenkins_slave` VM:
     ```bash
     vi ~/.ssh/authorized_keys
     ```

5. **Launch the Jenkins agent**:
   - After setting up the node, click **Launch agent** from the Jenkins master to connect the `jenkins_slave` VM as an agent.

6. **Set the number of executors**:
   - Set the number of executors for the built-in node (master) to **0** under **Manage Jenkins** → **Manage Nodes and Clouds** → **Built-in Node Configuration**.

---

## **Step 3: Tool Setup**

1. **Add Git in Jenkins**:
   - Navigate to **Manage Jenkins** → **Global Tool Configuration** → **Git**.
   - Set the Git executable path to `/usr/bin/git` (or the appropriate path based on your setup).

---

## **Step 4: Docker Registry Setup**

1. **Set up a private Docker registry on the `docker_repo` VM**:
   - Run the following command:
     ```bash
     docker run -d -p 5000:5000 --restart always --name registry registry:2
     ```

---

## **Step 5: Project Setup in Jenkins**

1. **Create a new pipeline project**:
   - Go to **New Item** → **Pipeline Project**.
   - Select **Pipeline from SCM**.

2. **Jenkinsfile**:
   - Ensure the `Jenkinsfile` is available in your GitHub repository or SCM source from which Jenkins will pull the code.

---

## **Step 6: SSH Configuration**

1. **Install SSH Agent Plugin in Jenkins**:
   - Navigate to **Manage Jenkins** → **Manage Plugins** → Search and install **SSH Agent Plugin**.

2. **Add SSH Credentials for Docker VM**:
   - Go to **Manage Jenkins** → **Credentials** → **Global** → **Add Credentials**.
   - Select **SSH Username with private key**, use the private key from the Docker VM (`~/.ssh/id_rsa`), and save the credentials.

3. **Establish SSH connection between `jenkins_slave` and `docker` VM**:
   - Ensure the Jenkins slave VM can connect to the Docker VM via SSH using the credentials added in the previous step.

---

## **Common Errors Faced During CI/CD**

### **Error 1: Cloning Remote Repository Failure**
- **Error**: `error cloning remote repo 'origin' jenkins`.
- **Solution**: Ensure Git is installed on the node that builds the project.
  
---

### **Error 2: Permission Denied for Docker Socket**
- **Error**: `ERROR: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock`.
- **Solution**:
   ```bash
   sudo usermod -aG docker $USER
   sudo reboot

---

### **Error 3: HTTPS Client to HTTP Server Error**
- **Error**: `Get "https://{url}/v2/": http: server gave HTTP response to HTTPS client`.
- **Solution**:
   ```bash
   sudo vi /etc/docker/daemon.json
   # Add the following to the file
   # {
   #"insecure-registries": ["192.168.33.25:5000"]
   # }
   sudo rebootsudo systemctl restart docker
---

## **GitHub Repository**
- Check out the complete code and configuration files on GitHub
- [Django Project]([https://www.openai.com](https://github.com/Sreevedh/django_ecommerce_website))
