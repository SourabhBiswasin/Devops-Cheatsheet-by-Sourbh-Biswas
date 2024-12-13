# Cheat sheet links

https://quickref.me/kubernetes


Building Blocks of Kubernetes
Or K8s Service name
1) Pod
2) Service
3) ReplicaSet
4) ConfigMap
5) Secrets





## K8s Pod yml file Sample Template
```Yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-nginx-pod
  labels:
    app: myapp
spec:
  containers:
  - name: nginx-container
    image: nginx
```
## K8s Service yml file Sample Template

```Yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: myapp
  ports:
  - protocol: TCP
    port: 80
    nodePort: 30001
  type: NodePort
  ```
## K8s ReplicaSet yml file Sample Template

```Yaml
# If we have ReplicaSet yaml file then Pod yaml file in not needed
apiVersion: apps/v1       
kind: ReplicaSet          
metadata:
  name: nginx-replicaset  
spec:
  replicas: 3             
  selector:
    matchLabels:          
      app: myapp          
  template:               
    metadata:
      labels:
        app: myapp        
    spec:
      containers:
      - name: nginx       
        image: nginx
```
## K8s ConfigMap yml file Sample Template

```Yaml
# If we have ReplicaSet yaml file then Pod yaml file in not needed
apiVersion: v1
kind: ConfigMap
metadata:
  name: mycmwithyaml
data:
  key4: val4
  key2: val2
  key3: val3
```
## K8s Pod yml file Sample Template with ConfigMap

```Yaml
# If we have ReplicaSet yaml file then Pod yaml file in not needed
apiVersion: v1
kind: Pod
metadata:
  name: configmap-pod-2
spec:
  containers:
  - name: nginx
    image: nginx    
    envFrom:
    - configMapRef:
        name: folder-configmap
```