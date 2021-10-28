INSTALLATION

        Install Jenkins on an Ubuntu server alogn with the Java 11

        [https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-18-04]

        [https://www.jenkins.io/doc/book/installing/linux/]

        Install docker and ansible

## JENKINS PLUGINS

After we install the default plaugins, we need to install the follwoing plugins

## MAKEFILE INSTLLATION

1. Blue Ocean
2. Slack upload and Slack notifications
3. Aqua Microscanner (replace with Trivy)
4. AWS Steps
5. Docker

Aqua Microscanner

https://github.com/aquasecurity/microscanner#registering-for-a-token
https://www.youtube.com/watch?v=cCJhN9nl1NY
https://github.com/aquasecurity/trivy

# CREADENTIALS

AWS_CREDENTIAL
DOCKER_HUB

# GLOBAL TOOL CONFIGURATIONS

Git and Docker

# PROVIDE PERMISSION FOR JENKINS TO RUN DOCKER AND ANSIBLE

Jenkins Got permission denied while trying to connect to the Docker daemon socket permission denied

        ```
        sudo usermod -a -G docker jenkins
        ```

Reboot the instance and Restart the Jenkins

## WHERE JENKINS KEEP ITS GIT REPO ????

## MAMANGE CONTAINERS USING ANSIBLE

## CREATE EKS CLUSTER

```
  aws configure
```

`eksctl create cluster --name capstoneproject --version 1.21 --nodegroup-name standard-workers --node-type t2.micro \ --nodes 2 --nodes-min 1 --nodes-max 3 --region us-east-1 --managed`

## MINIMUM AMI FOR EKSCTL

https://eksctl.io/usage/minimum-iam-policies/


Use Ansible for the container management 
Check the Dockerfile security
