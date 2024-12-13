# Cheat sheet links

https://quickref.me/kubernetes


# Building Blocks of Kubernetes
Or K8s Service name

1. ## [Pod](#detailed-description-of-pod)
2) ## Service
3) ## ReplicaSet
4) ## ConfigMap
   - ### Create a ConfigMap from Literal Values
     ```bash
     # Create a ConfigMap named example-configmap with literal values
     kubectl create configmap example-configmap --from-literal=key1=value1 --from-literal=key2=value2   
     ```
   - ### Create a ConfigMap from a File
   ```bash   
     # Create a file named config.txt with the following content:
     key1=value1
     key2=value2
    ```
    ```bash
     # Create a ConfigMap named file-configmap from the file
     kubectl create configmap file-configmap --from-file=config.txt
    ```
    - ### Create a ConfigMap from an Environment File
   ```bash   
     # Create an environment file named env-config.env with the following content:
     ENV_VAR1=value1
     ENV_VAR2=value2
    ```
    ```bash
     # Create a ConfigMap named env-configmap from the environment file:
     kubectl create configmap env-configmap --from-env-file=env-config.env
    ```
    - ### Create a ConfigMap from Multiple Files
     Create multiple files:
    config1.txt with content:
    ```bash   
     key1=value1
    ```
    config2.txt with content:
    ```bash   
     key2=value2
    ```

    ```bash
     # Create a ConfigMap named multi-file-configmap from multiple files:
     kubectl create configmap multi-file-configmap --from-file=config1.txt --from-file=config2.txt
    ```
    - #### Delete command for already available configmap
    ```bash
     kubectl delete configmap <configmap_name>
    ```
    OR
    ```bash
     kubectl delete cm <configmap_name>
    ```


5) ## Secrets

## Detailed Description of Pod

## 1) K8s Pod yml file Sample Template
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
## 2) K8s Service yml file Sample Template

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
## 3) K8s ReplicaSet yml file Sample Template

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
## 4) K8s ConfigMap yml file Sample Template

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
## 5) K8s Pod yml file Sample Template with ConfigMap

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