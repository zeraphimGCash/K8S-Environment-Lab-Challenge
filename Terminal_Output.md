```bash

Initialising Kubernetes... done
controlplane $ kubectl create namespace development
namespace/development created
controlplane $ kubectl create namespace staging
namespace/staging created
controlplane $ kubectl create configmap app-config \
>   --from-literal=APP_ENV=development \
>   --namespace=development
configmap/app-config created
controlplane $ 
controlplane $ 
controlplane $ kubectl create configmap app-config \
>   --from-literal=APP_ENV=staging \
>   --namespace=staging
configmap/app-config created
controlplane $ 
controlplane $ 
controlplane $ controlplane $ controlplane $ 

controlplane: command not found
controlplane $ 
controlplane $ 
controlplane $ 
controlplane $ 
controlplane $ kubectl create secret generic app-secret \
>   --from-literal=DB_PASSWORD=devpassword123 \
>   --namespace=development
secret/app-secret created
controlplane $ 
controlplane $ 
controlplane $ kubectl create secret generic app-secret \
>   --from-literal=DB_PASSWORD=stgpassword123 \
>   --namespace=staging
secret/app-secret created
controlplane $ 
controlplane $ 
controlplane $ 
controlplane $ 
controlplane $ controlplane $ 
controlplane: command not found
controlplane $ kubectl get secrets -n development
NAME         TYPE     DATA   AGE
app-secret   Opaque   1      21s
controlplane $ kubectl get secrets -n staging
NAME         TYPE     DATA   AGE
app-secret   Opaque   1      12s
controlplane $ kubectl describe secret app-secret -n development
Name:         app-secret
Namespace:    development
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
DB_PASSWORD:  14 bytes
controlplane $ kubectl describe secret app-secret -n staging
Name:         app-secret
Namespace:    staging
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
DB_PASSWORD:  14 bytes
controlplane $ 
controlplane $ ls  
filesystem  snap
controlplane $ vim app-deployment-template.yaml
controlplane $ vim app-deployment-template.yaml
controlplane $ sed 's/PLACEHOLDER_NAMESPACE/development/' app-deployment-template.yaml > dev-deployment.yaml 
controlplane $ 
controlplane $ kubectl apply -f dev-deployment.yaml
deployment.apps/nginx-app created
controlplane $ kubectl expose deployment nginx-app \
>   --type=NodePort \
>   --name=nginx-service \
>   --port=80 \
>   --target-port=80 \
>   --namespace=development
service/nginx-service exposed
controlplane $ kubectl get pods -n development
NAME                         READY   STATUS    RESTARTS   AGE
nginx-app-65968468b6-fzs9g   1/1     Running   0          14s
controlplane $ kubectl get service -n development
NAME            TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
nginx-service   NodePort   10.102.238.204   <none>        80:31878/TCP   5s
controlplane $ 
controlplane $ kubectl exec -it nginx-service -n development -- /bin/bash
Error from server (NotFound): pods "nginx-service" not found
controlplane $ echo $APP_ENV

controlplane $ echo $DB_PASSWORD

controlplane $ kubectl exec -it "nginx-service" -n "development" -- /bin/bash
Error from server (NotFound): pods "nginx-service" not found
controlplane $ echo $APP_ENV

controlplane $ echo $DB_PASSWORD

controlplane $ 
controlplane $ kubectl get service -n development
NAME            TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
nginx-service   NodePort   10.102.238.204   <none>        80:31878/TCP   2m6s
controlplane $ kubectl get pods -n development
NAME                         READY   STATUS    RESTARTS   AGE
nginx-app-65968468b6-fzs9g   1/1     Running   0          3m34s
controlplane $ kubectl exec -it "nginx-app-65968468b6-fzs9g" -n "development" -- /bin/bash
echo $APP_ENV
echo $DB_PASSWORDroot@nginx-app-65968468b6-fzs9g:/# echo $APP_ENV
development
root@nginx-app-65968468b6-fzs9g:/# echo $DB_PASSWORD
devpassword123
root@nginx-app-65968468b6-fzs9g:/# sed -e 's/PLACEHOLDER_NAMESPACE/staging/' \
    -e 's/replicas: .*$/replicas: 2/' app-deployment-template.yaml > staging-deployment.yaml

kubectl apply -f staging-deployment.yaml
controlplane $ ls
app-deployment-template.yaml  dev-deployment.yaml  filesystem  snap
controlplane $ sed -e 's/PLACEHOLDER_NAMESPACE/staging/' \
>     -e 's/replicas: .*$/replicas: 2/' app-deployment-template.yaml > staging-deployment.yaml
controlplane $ 
controlplane $ kubectl apply -f staging-deployment.yaml
deployment.apps/nginx-app created
controlplane $ 
controlplane $ kubectl expose deployment nginx-app \
>   --type=NodePort \
>   --name=nginx-service \
>   --port=80 \
>   --target-port=80 \
>   --namespace=staging
service/nginx-service exposed
controlplane $ 
controlplane $ 
controlplane $ kubectl get pods -n staging
NAME                         READY   STATUS    RESTARTS   AGE
nginx-app-65968468b6-2qgqj   1/1     Running   0          19s
nginx-app-65968468b6-ndqkj   1/1     Running   0          19s
controlplane $ kubectl get service -n staging
NAME            TYPE       CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
nginx-service   NodePort   10.97.2.125   <none>        80:30323/TCP   10s
controlplane $ kubectl exec -it nginx-app-65968468b6-2qgqj -n staging -- /bin/bash
root@nginx-app-65968468b6-2qgqj:/# echo $APP_ENV
staging
root@nginx-app-65968468b6-2qgqj:/# echo $DB_PASSWORD
stgpassword123
root@nginx-app-65968468b6-2qgqj:/# ls  
bin   dev                  docker-entrypoint.sh  home  lib64  mnt  proc  run   srv  tmp  var
boot  docker-entrypoint.d  etc                   lib   media  opt  root  sbin  sys  usr
root@nginx-app-65968468b6-2qgqj:/# exit
exit
controlplane $ vim challenge-prod.yaml
controlplane $ kubectl create namespace production
namespace/production created
controlplane $ 
controlplane $ kubectl create configmap app-config \
>   --from-literal=APP_ENV=production \
>   --namespace=production
configmap/app-config created
controlplane $ 
controlplane $ kubectl create secret generic app-secret \
>   --from-literal=DB_PASSWORD=prdpassword123 \
>   --namespace=production
secret/app-secret created
controlplane $ 
controlplane $ 
controlplane $ kubectl apply -f production-deployment.yaml
error: the path "production-deployment.yaml" does not exist
controlplane $ kubectl apply -f challenge-prod.yaml
deployment.apps/nginx-app created
controlplane $ kubectl expose deployment nginx-app \
>   --type=NodePort \
>   --name=nginx-service \
>   --port=80 \
>   --target-port=80 \
>   --namespace=production
service/nginx-service exposed
controlplane $ 
controlplane $ kubectl get pods -n production
NAME                         READY   STATUS    RESTARTS   AGE
nginx-app-65968468b6-5jzzn   1/1     Running   0          21s
nginx-app-65968468b6-flbwn   1/1     Running   0          21s
nginx-app-65968468b6-jb842   1/1     Running   0          21s
controlplane $ kubectl get service -n production
NAME            TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
nginx-service   NodePort   10.99.222.159   <none>        80:30857/TCP   9s
controlplane $ 
controlplane $ kubectl exec -it nginx-app-65968468b6-5jzzn -n production -- /bin/bash
root@nginx-app-65968468b6-5jzzn:/# echo $APP_ENV
production
root@nginx-app-65968468b6-5jzzn:/# echo $DB_PASSWORD
prdpassword123
root@nginx-app-65968468b6-5jzzn:/# 
```