## Install Argo CD Windows
```
Step1: 
Start Docker Desktop

Step2:
Kubernetes is Running

Step3:
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

Step4:
kubectl get pods -n argocd

Step5: get password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"

Step6: Decode Password
https://www.base64decode.org/

Step7:
kubectl port-forward svc/argocd-server -n argocd 8080:443

Step8:
http://localhost:8080
username: admin
password: sdBxZrkPLTQFuPHY
```
## Delete App 
```
Delete All App's In GUI & Delete Argo CD
```
## Delete Argo CD
```
kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Application Deployment using Argo CD
Reffer this [Link](https://drive.google.com/file/d/1aZ2jUXxPiNGf_buJnUC6I_t7ww7upPG2/view?usp=sharing) to see GUI Steps to Deploy Tetris game in K8s Cluster