# Deploying an application on Kubernetes

## Prerequisites
> - [Docker](https://www.docker.com/products/docker-desktop/) installed 
> - [Node.js](https://nodejs.org/en/download/) installed 

## Install
```
git clone https://github.com/HrushikeshKaranth/kubernetes-deployment.git
```
```
cd kubernetes-deployment
```
```
npm install
```

## Run
```
node app.js
```
Visit http://localhost:3000 in your browser

## Test
To run tests
```
npm test
```

## Containerize the application using Dockerfile
```
FROM node:14-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD [ "npm", "start" ]
```

## Build and Push code to Docker hub
```
docker build -t $your docker hub user name/$image name .
docker push $your docker hub user name/$image name .
```

## Create Kubernetes manifest file
deployment.yaml file
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kubernetes-deployment
  labels:
    app: kubernetes-deployment-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: notes-app
  template:
    metadata:
      labels:
        app: notes-app
    spec:
      containers:
      - name: kubernetes-deployment-app
        image: hrushikesh0/kubernetes-deployment-app
        resources:
          requests:
            cpu: "100m"
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 3000
```
service.yaml
```
apiVersion: v1
# Indicates this as a service
kind: Service
metadata:
 # Service name
 name: kubernetes-deployment-service
spec:
 selector:
   # Selector for Pods
   app: kubernetes-deployment-app
 ports:
   # Port Map
 - port: 80
   targetPort: 3000
   protocol: TCP
 type: LoadBalancer
```

## Apply the manifest files
```
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl get pods
```
You should see the pods and their status.

