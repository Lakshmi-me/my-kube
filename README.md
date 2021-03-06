# my-kube

minikube cache add gcr.io/k8s-prow/hook:latest

minikube cache add gcr.io/k8s-prow/sinker:latest

minikube cache add gcr.io/k8s-prow/deck:latest

minikube cache add gcr.io/k8s-prow/horologium:latest

minikube cache add gcr.io/k8s-prow/tide:latest

minikube cache add gcr.io/k8s-prow/status-reconciler:latest

minikube cache add gcr.io/k8s-prow/ghproxy:latest

minikube cache add gcr.io/k8s-prow/prow-controller-manager:latest

minikube cache add gcr.io/k8s-prow/crier:latest

### enable the following addons

minikube addons enable metrics-server

minikube addons enable dashboard

minikube addons enable volumesnapshots

minikube addons enable csi-hostpath-driver

minikube addons enable ingress

### configure github repo webhook

generate github webhook secret:

openssl rand -hex 20

Create a github bot user account and add a PAT with full access. Note down the generated PAT

### deploy prow yaml

kubectl create clusterrolebinding cluster-admin-binding-"${USER}" --clusterrole=cluster-admin --user="${USER}"

kubectl apply -f starter-s3.yaml

### strategic merge patch

F:\my-kube>type hmac-token.yaml
stringData:
  hmac: abcd

F:\my-kube>type c:\github-token.yaml
stringData:
  token: abcd

minikube kubectl -- patch secret hmac-token -n prow --patch $(Get-Content C:\hmac-token.yaml -Raw)

minikube kubectl -- patch secret github-token -n prow --patch $(Get-Content C:\github-token.yaml -Raw)

at this point verify that all prow pods are up and running

### ultrahook to forward github events to local minikube instance

sign up for ultrakook. You'll recieve a api key in your email
Place the contents of the api key in ".ultrahook" file in the current user's home dir
install ruby
sudo gem install ultrahook

minikube ip

add minikube ip to etc hosts like :

"172.17.185.40 prow.nmam.com"

kubectl get svc -n prow (get the nodeport)

ultrahook github http://192.168.21.101:31931/hook

ultrahook github http://prow.nmam.com/hook

### To access the dashboard
 
kubectl proxy

http://127.0.0.1:8001/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/#/overview?namespace=default

### To access prow deck (and hook)

http://prow.nmam.com/  -- should show the prow PR dashboard

http://prow.nmam.com/hook   -- should give method not found

### online yaml linter
https://jsonformatter.org/yaml-validator