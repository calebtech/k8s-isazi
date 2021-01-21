# k8s-isazi dummy app
To run this dummy app successfully follow the instructions below
    git clone https://github.com/calebtech/k8s-isazi.git
    cd k8s-isazi/
### Pre-requisites
- Ubuntu 16.04 or 18.04 VM 
- docker must be running and current user is added to the docker group with `sudo usermod -aG docker $USER`
- kubetctl must be running 
- minikube must be running, preferred minikube version: v1.10.0 can be found [here](https://github.com/kubernetes/minikube/releases/tag/v1.10.0) and start your minikube cluster with `minikube start --driver=none` so you can the cluster locally
- ansible must be running preferred version 2.9, note ansible depends on python, on this case our ansible is built over python 2.7 you can use `ansible --version | grep "python version"` to check the python version ansible uses
- ansible-galaxy collection install community.kubernetes since we are provisioning a k8s cluster 
- openshift must be installed

## A walk through installation to the dependencies for the k8s dummy app
### Starting minikube
We are running a ubuntu vm so we will use `--driver=none`

    sudo minikube start --driver=none

### Enable ingress for later use
    sudo minikube addons enable ingress

### Install ansble 2.9
    sudo apt install software-properties-common
    sudo apt-add-repository ppa:ansible/ansible
    sudo apt update
    sudo apt install ansible

#### Installing the Collection from Ansible Galaxy

Before using the Kubernetes collection, you need to install it with the Ansible Galaxy CLI:

    ansible-galaxy collection install community.kubernetes

### Installing the OpenShift Python Library

Content in this collection requires the [OpenShift Python client](https://pypi.org/project/openshift/) to interact with Kubernetes' APIs. You can install it with:

    pip install openshift

### Now let's run our ansible script to provision our cluster
    ansible-playbook main.yaml

### Check if your pods are running
    kubectl get pods --all-namespaces

### Now let's send the traffic to the ingress HOST, we have a mini script that does that for us
    sudo ./minikube-hosts-update

Output:

   minikube ip hello-isazi.co

Verify that the Ingress controller is directing traffic:

curl hello-isazi.co


