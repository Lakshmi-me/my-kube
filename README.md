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

minikube addons enable dashboard
minikube addons enable ingress

sudo gem install ultrahook

minikube ip

add minikube ip to etc hosts like "172.17.185.40 prow.nmam.com"

kubectl get svc -n prow (get the nodeport)

ultrahook github http://192.168.21.101:31931/hook

ultrahook github http://prow.nmam.com/hook

kubectl delete namespace prow test-pods

kubectl create clusterrolebinding cluster-admin-binding-"${USER}" --clusterrole=cluster-admin --user="${USER}"

kubectl apply -f starter-s3.yaml

### To access the dashboard

kubectl proxy

http://prow.nmam.com:8001/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/#/overview?namespace=default

### To access prow deck (and hook)

http://prow.nmam.com/  -- should show the prow PR dashboard
http://prow.nmam.com/hook   ---- should give method not found

### strategic merge patch

F:\my-kube>type patch.yaml
stringData:
  token: 4606953fcabfa2024fa33b8a29ce1386a4057d60

PS C:\Windows\system32> minikube kubectl -- patch secret github-token -n prow --patch $(Get-Content F:\my-kube\patch.yaml -Raw)
secret/github-token patched