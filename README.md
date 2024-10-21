Jenkins-Docker CI/CD Pipeline
This project demonstrates how to set up a CI/CD pipeline using Jenkins and Docker across multiple virtual machines (VMs). The pipeline automates the process of building, pushing, and deploying Docker images.

Step 1: VM Setup
Create four VMs: docker, jenkins, jenkins_slave, and docker_repo (I used Vagrant with CentOS OS).

Install necessary components:

Jenkins and Docker on jenkins and jenkins_slave VMs.
Docker on docker and docker_repo VMs.
Git on both jenkins and jenkins_slave VMs.
Update all VMs:

bash
Copy code
sudo yum update -y
Access Jenkins UI on your browser at localhost:8080, install required plugins, and sign in.
