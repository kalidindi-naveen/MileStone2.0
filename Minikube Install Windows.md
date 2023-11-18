## Install Docker Desktop
https://medium.com/womenintechnology/create-a-3-node-kubernetes-cluster-with-minikube-8e3dc57d6df2

## Install Minikube
https://minikube.sigs.k8s.io/docs/start/

### PowerShell Run as Administrator
```
New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing
```

### Install Kubectl
https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/
```
- Add Path to Env Variables
kubectl version --client
```

### Minikube Commands
```
minikube start // 1 node
```
### Minikube with 2 Nodes
```
minikube start --nodes 2 -p twonodes // -p means profile
minikube tunnel -p twonodes

kubectl get nodes
NAME           STATUS   ROLES           AGE     VERSION
twonodes       Ready    control-plane   4m      v1.25.3
twonodes-m02   Ready    <none>          2m46s   v1.25.3

minikube status -p twonodes
twoNodes
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

twoNodes-m02
type: Worker
host: Running
kubelet: Running
```
minikube pause
minikube unpause
minikube stop
minikube addons list
minikube addons enable ingress // for local testing
minikube dashboard // web ui
- Delete all of the minikube clusters:
minikube delete --all
```