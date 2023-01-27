# awx-on-ubuntu-Minikube
# MiniKube installation

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube

sudo usermod -aG docker $USER && newgrp docker

minikube start --driver=docker

alias kubectl="minikube kubectl --"

kubectl get po -A

minikube addons list

minikube start --addons=ingress

minikube addons enable metrics-server

minikube dashboard

# open a second terminal 

firefox

should have access to dashboard

# Install Kustomize

curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh" | bash

sudo cp kustomize /usr/local/bin

# Install the manifests
# You will need two files kustomization.yaml & awx-demo.yaml

kustomize build . | kubectl apply -f -

# checking installation process

kubectl logs -f deployments/awx-operator-controller-manager -c awx-manager

# Check to make sure operator begin to create new resources

kubectl get pods -l "app.kubernetes.io/managed-by=awx-operator"

kubectl get svc -l "app.kubernetes.io/managed-by=awx-operator"

# The AWX instance will be accessible by running:

minikube service -n awx awx-demo-service --url

# Username is admin and run the following command to get the password for the AWX portal

kubectl get secret awx-demo-admin-password -o jsonpath="{.data.password}" | base64 --decode ; echo

