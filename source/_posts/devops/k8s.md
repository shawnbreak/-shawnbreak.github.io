# K8S

## Layer of Abstraction

- deployment

- replicaset

- pod

- container



```sh
kubectl get nodes
kubectl get pod
kubectl create deployment NAME --iamge=image-name
kubectl get deployment
kubectl get replicaset
kubectl edit deployment NAME
kubectl describe pod [pod-name]
kubectl exec -it [pod-name] -- /bin/bash
kubectl delete deployment deployment-name
kubectl apply -f config-file.yml
kubectl delete -f config-file.yml
```



## Configuration YAML File

- metadata
- specification
- status



Lables & Selectors



## minikube

```sh
minikube version
kubectl version
kubectl cluster-info
kubectl get nodes
kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
kubectl get deployment
kubectl get replicaset
kubectl get pod
kubectl proxy


```

- **kubectl get** - list resources
- **kubectl describe** - show detailed information about a resource
- **kubectl logs** - print the logs from a container in a pod
- **kubectl exec** - execute a command on a container in a pod

```sh
kubectl get services
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
service/kubernetes-bootcamp exposed

kubectl describe services/kubernetes-bootcamp
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')

curl $(minikube ip):$NODE_PORT
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-765bf4c7b4-x966z | v=1
```

