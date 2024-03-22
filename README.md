# Deploying microservices

## How to Install Kubernetes on Ubuntu 22.04

## How to disable cloud-init in Ubuntu

### Prevent start
* Create an empty file to prevent the service from starting
 
      sudo touch /etc/cloud/cloud-init.disabled
  
### Uninstall
* Disable all services (uncheck everything except "None"):

      sudo dpkg-reconfigure cloud-init
  
* Uninstall the package and delete the folders

      sudo dpkg-reconfigure cloud-init
      sudo apt-get purge cloud-init
      sudo rm -rf /etc/cloud/ && sudo rm -rf /var/lib/cloud/
  
* Restart the computer

      sudo reboot

### Windows Client, GitLab Community Server, Jenkins & Docker
| Server Role             | Server Hostname           | Specs                                             | IP Address     | Host Port |
| ----------------------- | ------------------------- | ------------------------------------------------- | -------------- | --------- |
| Windows Clinet          | windev                    | 2 vCPU, 4 GB RAM, 100GB Disk EACH                 | 192.168.15.10  | 22010     |
| GitLab Community Server | gitlab                    | 2 vCPU, 4 GB RAM, 100GB Disk EACH                 | 192.168.15.20  | 22020     |
| Jenkins & Docker        | jeknis                    | 2 vCPU, 4 GB RAM(or 4 vCPU, 8GB), 100B Disk EACH  | 192.168.15.30  | 22030     |

### Install Commons
sudo apt install net-tools vim nano iputils-ping netcat openssh-server iputils-ping

### Windows Client
* Windows 10 or 11 Pro
* JDK 17 higher
  - https://www.oracle.com/kr/java/technologies/javase/javase7-archive-downloads.html
* Maven
  - https://maven.apache.org/
* Environment Variables
  - JAVA_HOME, MAVEN_HOME or Gradle
* Gradle
  - https://github.com/gradle/gradle
* STS
  - https://spring.io/tools
* STS Plugin
  - https://marketplace.eclipse.org/content/eclipse-enterprise-java-and-web-developer-tools#details
* Lombok
  - https://projectlombok.org/download
  
### GitLab Community Server
https://about.gitlab.com/install/#ubuntu

      sudo apt-get update
      sudo apt-get install -y curl openssh-server ca-certificates tzdata perl
      curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash

### Jenkins
https://www.jenkins.io/doc/book/installing/linux/
    
* Installation of Java
  
      sudo apt update
      sudo apt install fontconfig openjdk-17-jre
      java -version
      openjdk version "17.0.8" 2023-07-18
      OpenJDK Runtime Environment (build 17.0.8+7-Debian-1deb12u1)
      OpenJDK 64-Bit Server VM (build 17.0.8+7-Debian-1deb12u1, mixed mode, sharing)
  
* Long Term Support release
  
      sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
        https://pkg.jenkins.io/debian/jenkins.io-2023.key
      echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
        https://pkg.jenkins.io/debian binary/ | sudo tee \
        /etc/apt/sources.list.d/jenkins.list > /dev/null
      sudo apt-get update
      sudo apt-get install jenkins

### Docker
https://docs.docker.com/engine/install/ubuntu/

* Install using the apt repository

      # Add Docker's official GPG key:
      sudo apt-get update
      sudo apt-get install ca-certificates curl
      sudo install -m 0755 -d /etc/apt/keyrings
      sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
      sudo chmod a+r /etc/apt/keyrings/docker.asc
      
      # Add the repository to Apt sources:
      echo \
        "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
        $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
        sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
      sudo apt-get update

      sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
      sudo docker run hello-world
 
* Post-installation steps

      sudo groupadd docker
      sudo usermod -aG docker $USER
      # Log out and log back
      newgrp docker
      docker run hello-world

      // Security Issues 
      sudo chmod 666 /var/run/docker.sock or sudo chown root:docker /var/run/docker.sock
      sudo usermod -a -G docker jenkins

### Jenkins & Docker

      sudo apt install git maven

### Service provided by Kubernetes
* Self-healing
* Horizontal scaling
* Compute scheduling
* Service discovery/load balancing
* Automated rollouts & rollbacks
* Secret & configuration management
* Volume management

### Kubernetes Nodes
* Master Nodes : This handles the control API calls for the pods, replications controllers, services, nodes and other components of a Kubernetes cluster.
* Node : Provides the run-time environments for the containers. A set of container pods can span multiple nodes.

* Master/Worker Nodes : minimum hardware requirements recommendation.
  - Memory: 2 GiB or more of RAM per machine
  - CPUs: At least 2 CPUS
  - Internet connectivity or Not
  - Full network connectivity between machines in the cluster

* Links
  - UBUNTU SERVER LTS 22.04.3 - https://ubuntu.com/download/server
  - KUBERNETES 1.29.1         - https://kubernetes.io/releases/
  - CONTAINERD 1.7.13         - https://containerd.io/releases/
  - RUNC 1.1.12               - https://github.com/opencontainers/runc/releases
  - CNI PLUGINS 1.4.0         - https://github.com/containernetworking/plugins/releases
  - CALICO CNI 3.27.2         - https://docs.tigera.io/calico/3.27/getting-started/kubernetes/quickstart

### Installing Kubernetes Cluster on Ubuntu 22.04 using kubeadm
| Server Role    | Server Hostname           | Specs                             | IP Address     | Host Port |
| -------------- | ------------------------- | --------------------------------  | -------------- | --------- |
| Master Node    | k8s-contro                | 2 vCPU, 4 GB RAM, 100GB Disk EACH | 192.168.15.93  | 22093     |
| Worker Node #1 | k8s-1                     | 2 vCPU, 4 GB RAM, 100GB Disk EACH | 192.168.15.94  | 22094     |
| Worker Node #2 | k8s-2                     | 2 vCPU, 4 GB RAM, 100B Disk EACH  | 192.168.15.95  | 22095     |

### Upgrade Ubuntu servers

    sudo apt update
    sudo apt -y full-upgrade && sudo reboot -f  

### Install Cluster Kubernetes 1.29.x on Ubuntu 22.04
[https://computingforgeeks.com/install-kubernetes-cluster-ubuntu-jammy/](https://www.youtube.com/watch?v=I9goyp8mWfs)

    ### ALL: 
    
    sudo su
    
    printf "\n192.168.15.93 k8s-control\n192.168.15.94 k8s-1\n192.168.15.95 k8s-1\n192.168.15.30 jenkins\n192.168.15.20 gitlab\n192.168.15.10 windev\n\n" >> /etc/hosts
    
    printf "overlay\nbr_netfilter\n" >> /etc/modules-load.d/containerd.conf
    
    modprobe overlay
    modprobe br_netfilter
    
    printf "net.bridge.bridge-nf-call-iptables = 1\nnet.ipv4.ip_forward = 1\nnet.bridge.bridge-nf-call-ip6tables = 1\n" >> /etc/sysctl.d/99-kubernetes-cri.conf
    
    sysctl --system
    
    wget https://github.com/containerd/containerd/releases/download/v1.7.13/containerd-1.7.13-linux-amd64.tar.gz -P /tmp/
    tar Cxzvf /usr/local /tmp/containerd-1.7.13-linux-amd64.tar.gz
    wget https://raw.githubusercontent.com/containerd/containerd/main/containerd.service -P /etc/systemd/system/
    systemctl daemon-reload
    systemctl enable --now containerd
    
    wget https://github.com/opencontainers/runc/releases/download/v1.1.12/runc.amd64 -P /tmp/
    install -m 755 /tmp/runc.amd64 /usr/local/sbin/runc
    
    wget https://github.com/containernetworking/plugins/releases/download/v1.4.0/cni-plugins-linux-amd64-v1.4.0.tgz -P /tmp/
    mkdir -p /opt/cni/bin
    tar Cxzvf /opt/cni/bin /tmp/cni-plugins-linux-amd64-v1.4.0.tgz
    
    mkdir -p /etc/containerd
    containerd config default | tee /etc/containerd/config.toml   <<<<<<<<<<< manually edit and change SystemdCgroup to true (not systemd_cgroup)
    vi /etc/containerd/config.toml
    systemctl restart containerd
    
    swapoff -a  <<<<<<<< just disable it in /etc/fstab instead
    
    apt-get update
    apt-get install -y apt-transport-https ca-certificates curl gpg
    
    mkdir -p -m 755 /etc/apt/keyrings
    curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
    echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
    
    apt-get update
    
    reboot
    
    sudo su
    
    apt-get install -y kubelet=1.29.1-1.1 kubeadm=1.29.1-1.1 kubectl=1.29.1-1.1
    apt-mark hold kubelet kubeadm kubectl
    
    # check swap config, ensure swap is 0
    free -m
    
    
    ### ONLY ON CONTROL NODE .. control plane install:
    kubeadm init --pod-network-cidr 10.10.0.0/16 --kubernetes-version 1.29.1 --node-name k8s-control
    
    export KUBECONFIG=/etc/kubernetes/admin.conf
    
    # add Calico 3.27.2 CNI 
    kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.2/manifests/tigera-operator.yaml
    wget https://raw.githubusercontent.com/projectcalico/calico/v3.27.2/manifests/custom-resources.yaml
    vi custom-resources.yaml <<<<<< edit the CIDR for pods if its custom
    kubectl apply -f custom-resources.yaml
    
    # get worker node commands to run to join additional nodes into cluster
    kubeadm token create --print-join-command
    ###
    
    
    ### ONLY ON WORKER nodes
    Run the command from the token create output above
    
### Kubernetes Cluster Nodes Access Example

    To start using your cluster, you need to run the following as a regular user:
    
    mkdir -p $HOME/.kube
      sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
      sudo chown $(id -u):$(id -g) $HOME/.kube/config
    
    Alternatively, if you are the root user, you can run:
    
      export KUBECONFIG=/etc/kubernetes/admin.conf
    
    You should now deploy a pod network to the cluster.
    Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
      https://kubernetes.io/docs/concepts/cluster-administration/addons/
    
    Then you can join any number of worker nodes by running the following on each as root:
    
    kubeadm join 192.168.10.101:6443 --token i6we06.lysnlhipdt82tg3z \
            --discovery-token-ca-cert-hash sha256:f06fb5d38bbebd754fda7ae1944ebfe424241d6961acecfc4640aa60bccf0253


### Kubernetes Dashboard
https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md
https://k21academy.com/docker-kubernetes/kubernetes-dashboard/
