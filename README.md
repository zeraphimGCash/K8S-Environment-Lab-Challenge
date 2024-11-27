# K8S-Environment-Lab-Challenge

Challenge:
Create a deployment for production environment. Set also the replicas to 3. Kindly share the code as markdown file on github

## Step 1: Creating a YAML for production environment

Name: Challenge-Prod-JCDiamante.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-app
  namespace: production
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-app
  template:
    metadata:
      labels:
        app: nginx-app
    spec:
      containers:
      - name: nginx
        image: nginx
        env:
        - name: APP_ENV
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: APP_ENV
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: app-secret
              key: DB_PASSWORD
```

## Step 2: Applying the production configuration

Enter this in terminal

```bash
kubectl create namespace production

kubectl create configmap app-config \
  --from-literal=APP_ENV=production \
  --namespace=production

kubectl create secret generic app-secret \
  --from-literal=DB_PASSWORD=prdpassword123 \
  --namespace=production
```

## Step 3: Deployment

Enter this in terminal

```bash
kubectl apply -f Challenge-Prod-JCDiamante.yaml
```

## Step 4: Exposing the production service
```bash
kubectl apply -f production-deployment.yamlkubectl expose deployment nginx-app \
  --type=NodePort \
  --name=nginx-service \
  --port=80 \
  --target-port=80 \
  --namespace=production
```

## Step 5: Verifying the deployment

Enter this in terminal

```bash
kubectl get pods -n production
kubectl get service -n production
```

Accessing the environment variables

```bash
kubectl exec -it [POD_NAME] -n production -- /bin/bash
echo $APP_ENV
echo $DB_PASSWORD
```
