# jenkins-docker-cicd-pipeline
# STEP 1 : 
- Up four vm - docker, Jenkins, jenkins_slave, docker_repo ( i used vagrant to up the vm's)
- install jenkins, docker in jenkins and jenkins_slave
- install git in jenkins and jenkins slave
- install docker in docker and docker repo

- do "sudo yum update -y" for all vms

- open the jenkins master ui ( localhost:8080 ) and install plugins and sign in

## STEP 2
      #### Setting up jenkins slave
    - create a vm, create a user ( or keep the user as it is). install jenkins.
    - do `ssh-keygen -t rsa` command, copy the `private key` and paste it in the “new node” configuration.
    - SELECT NONE HOST VERIFICATION STRATEGY UNDER VERFICATION IN NODE CONFIGURATION.
    - after this make sure to copy the public key and paste it in the “authorized_keys”  file inside “.ssh”
    - launch the agent
    - SET number of executors for built in node to 0.

    for tool to set up.---> have to look later
    - Add the git path in the tools section in the slave configuration

## STEP 3 
 - https://hub.docker.com/_/registry
 - $ docker run -d -p 5000:5000 --restart always --name registry registry:2

## STEP 4:
new project -> pipeline project -> select pipeline from scm.
make sure you have Jenkinsfile in GitHub or the scm you are pulling the code from.

## GitHub Repo
https://github.com/Sreevedh/django_ecommerce_website




## SSH STEP 
 - install ssh agent plugin
 - manage jenkins -> credential -> add credential for the docker vm by username with ssh and enter the private key ( create using ssh-keygen -t rsa, cat ~/.ssh/id_rsa in the docker vm you intend jenkins to connect to)
- added ssh between jenkins_slave and docker vm


## Errors faced during ci/cd:

#### Error 1
- error error cloning remote repo 'origin' jenkins
    #### Solution
- ensure the node using for building have git installed in the vm.

#### Error 2
 - ERROR: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Head "http://%2Fvar%2Frun%2Fdocker.sock/_ping": dial unix /var/run/docker.sock: connect: permission denied
   #### Solution
- sudo usermod -aG docker $USER
- sudo reboot

#### Error 3
- Get "https://192.168.33.25:5000/v2/": http: server gave HTTP response to HTTPS client
  #### Solution
- sudo vi /etc/docker/daemon.json 
- insert '{
  "insecure-registries" : ["192.168.33.25:5000"]
} '
- sudo systemctl restart docker


 
