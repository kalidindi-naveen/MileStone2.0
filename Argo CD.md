### Pre-Requisites
- Create EC2 ubuntu → t2.medium
- Install K8s locally (Docker + K3D (K8s in Docker) as it support RBAC)\
- Install Kubectl

## Local Development K3s
→ K3s is a lighter version of K8, which has more extensions and drivers. \
→ K8s often takes 10 minutes to deploy, K3s can execute the Kubernetes API in as little as one minute\
→ K3s is faster to start up, and is easier to auto-update and learn \
→ k3d makes it very easy to create single- and multi-node k3s clusters in docker\
→ e.g. for local development on Kubernetes (Master & Worker Nodes will run as Docker Container's)

## Install Docker

## Install K3D
https://k3d.io/v5.6.0/
```
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash

root@ip-172-31-89-236:~# k3d version
k3d version v5.6.0
k3s version v1.27.4-k3s1 (default)
```

## Install Kubectl
https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
```
root@ip-172-31-90-204:~# curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

root@ip-172-31-90-204:~# sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

root@ip-172-31-90-204:~# kubectl version --client
Client Version: v1.28.2
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
```
## Create Cluster name : mcd1
```
k3d cluster create mcd1
```
```
root@ip-172-31-90-204:~# k3d cluster list
NAME   SERVERS   AGENTS   LOADBALANCER
mcd1   1/1       0/0      true

root@ip-172-31-90-204:~# kubectl cluster-info
Kubernetes control plane is running at https://0.0.0.0:46849
CoreDNS is running at https://0.0.0.0:46849/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://0.0.0.0:46849/api/v1/namespaces/kube-system/services/https:metrics-server:https/proxy
```
```
root@ip-172-31-90-204:~# k3d node list
NAME                ROLE           CLUSTER   STATUS
k3d-mcd1-server-0   server         mcd1      running
k3d-mcd1-serverlb   loadbalancer   mcd1      running
```
```
root@ip-172-31-90-204:~# docker ps
CONTAINER ID   IMAGE                            COMMAND                  CREATED         STATUS         PORTS                             NAMES
6295f4828619   ghcr.io/k3d-io/k3d-proxy:5.6.0   "/bin/sh -c nginx-pr…"   3 minutes ago   Up 3 minutes   80/tcp, 0.0.0.0:46849->6443/tcp   k3d-mcd1-serverlb
e376723dc933   rancher/k3s:v1.27.4-k3s1         "/bin/k3d-entrypoint…"   3 minutes ago   Up 3 minutes                                     k3d-mcd1-server-0
```
```
k3d cluster stop mcd1
k3d cluster start mcd1
k3d cluster delete mcd1
```
## MultiNode Cluster
```
root@ip-172-31-90-204:~# k3d cluster create mcd1 --agents 2 --servers 1 
→ 2 Workers 1 Master

root@ip-172-31-90-204:~# k3d node list
NAME                ROLE           CLUSTER     STATUS
k3d-mcd1-agent-0    agent          mcd1   running
k3d-mcd1-agent-1    agent          mcd1   running
k3d-mcd1-server-0   server         mcd1   running
k3d-mcd1-serverlb   loadbalancer   mcd1   running
```
## Add Agent to mcd1 cluster
```
root@ip-172-31-90-204:~# k3d node create agent007 --role agent --cluster mcd1

root@ip-172-31-90-204:~# k3d node list
NAME                ROLE           CLUSTER     STATUS
k3d node create agent007 --role agent --cluster mcd1
k3d-mcd1-agent-0    agent          mcd1   running
k3d-mcd1-agent-1    agent          mcd1   running
k3d-mcd1-server-0   server         mcd1   running
k3d-mcd1-serverlb   loadbalancer   mcd1   running
```
## Install Argo CD AWS
https://kubebyexample.com/learning-paths/argo-cd/argo-cd-getting-started
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
```
kubectl get pods -n argocd
```
```
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

8GHzbx1LD23rrS0r
```
## Delete Argo CD
```
kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```