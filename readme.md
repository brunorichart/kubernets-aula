
Cópia
curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.32.0/kind-windows-amd64
Move-Item .\kind-windows-amd64.exe C:\infra\kind.exe


kind 
kind version
kind create cluster --name cluster2

kubectl cluster-info --context kind-mycluster
kind delete clusters kind-mycluster
kubectl get nodes 
kind create cluster --name mycluster --config C:\infra\clusters\clusters.yml

kubectl api-resources | less
kubectl api-resources   -- pegar todos os recursos disponíveis 

kubectl create -f C:\infra\clusters\pod.yml
kubectl get pods
kubectl get pods -o wide
kubectl delete pod myapp

kubectl get namespaces
kubectl get pods -n kube-system -- pegar informações dos pods de administração 
kubectl get all

kubectl api-resources | grep -i deployment

kubectl create -f C:\infra\clusters\deployment.yml
watch -n1 kubectl get pods -o wide
kubectl delete pod mydeploy-5cb6f486d8-kj679
kubectl delete pod mydeploy-5cb6f486d8-98vpm


kubectl explain deployment
kubectl explain deployment.metadata
kubectl explain deployment.spec.template 

kubectl get deployment
kubectl get all
kubectl get all -o wide

# Executar um pod
kubectl exec -ti pod/myapp -- bash
curl 10.244.2.3   -- executar um curl dentro do container

kubectl api-resources | grep service

# Criando serviço
kubectl expose deployment mydeploy --port=8080 --target-port=80 --protocol=TCP
kubectl get all -o wide
kubectl get endpoints
kubectl exec -ti pod/myapp -- bash
curl 10.244.1.6
curl mydeploy:8080
curl mydeploy.default:8080  -- passando o namespace 
cat /etc/resolv.conf
kubectl get svc -n kube-system  -- aqui que roda o serviço DNS

# Trabalhando com namespace
kubectl create namespace dev
kubectl get pods -n dev
kubectl create -f C:\infra\clusters\deployment.yml -n dev
kubectl get pods -n dev
kubectl expose deployment mydeploy --port=8080 --target-port=80 --protocol=TCP -n dev
kubectl get svc -n dev
kubectl get endpoints -n dev
kubectl exec -ti pod/myapp -- bash
curl mydeploy.default:8080
curl mydeploy.dev:8080

# Tipos de serviços
kubectl get svc
kubectl get service mydeploy -o yaml
kubectl edit service mydeploy


# Replicas 
kubectl get deployment
replicas: 3
kubectl create -f C:\infra\clusters\deployment.yml --overwrite
kubectl apply -f C:\infra\clusters\deployment.yml
kubectl get deployment,pod
kubectl get endpoints
kubectl get pods -o wide
kubectl edit deployment mydeploy
kubectl scale --replicas 3 deployment mydeploy


# StateFulSet
kubectl apply -f C:\infra\clusters\statefulset.yml
kubectl delete pod mystatefulset-1

# Variáveis de ambiente
kubectl apply -f C:\infra\clusters\mydb.yml
kubectl get pods


# Criando variáveis de ambiente
kubectl get deployment
kubectl logs -f mydb-c76d5f5fb-l8p7b
kubectl delete -f C:\infra\clusters\mydb.yml 

# Secret
kubectl get pods
kubectl delete -f C:\infra\clusters\mydb.yml 
kubectl create secret generic mysecret --from-literal MYSQL_ROOT_PASSWORD=mypass --from-literal MYSQL_DATABASE=mydb --from-literal MYSQL_USER=wordpress --from-literal MYSQL_PASSWORD=wppass
kubectl get secrets
echo "d29yZHByZXNz" | base64 -d

