# Cheat sheet links

https://quickref.me/kubernetes


# Core resources of Kubernetes
Or K8s Service name

1. ## [Pod](#detailed-description-of-pod)
2) ## [Service](#detailed-description-of-service)
3) ## [ReplicaSet](#detailed-description-of-replicaset)
4) ## [ConfigMap](#detailed-description-of-configmap)
5) ## [Secrets](#detailed-description-of-secrets)
6) ## [Deployment](#detailed-description-of-deployment) 
7) ## [Deployment](#detailed-description-of-deployment) 


## Detailed Description of Pod
Pod represents a single instance/server of a running process in your cluster and can contain one or more containers.

### K8s Pod yml file to create Pod inside k8s

Command to apply yaml in k8s. It can create new resources if they don’t exist or update existing resources to match the configuration in the file.
```bash
kubectl apply -f <name_of_file>
```

Sample Template

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


## Detailed Description of Service
Service work on top of Pod. Services enable communication between different components in a Kubernetes cluster, allowing stable networking for Pods even if they are ephemeral(pod are down) and their IP addresses change.

### K8s Service yml file to create Service inside k8s
Sample Template

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
## Detailed-Description-Of-Replicaset
ReplicaSet ensures a specified number of identical Pods are running at any given time. It provides high availability and scaling by automatically creating or deleting Pods to maintain the desired state.

## K8s ReplicaSet yml file to create Replicaset inside k8s
Sample Template

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
## Detailed-Description-Of-Configmap

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
<<<<<<< HEAD
     kubectl create configmap <configmap_name> --from-file=config1.txt --from-file=config2.txt
    ```
    - #### Delete command for already available configmap
    ```bash
=======
     kubectl create configmap multi-file-configmap --from-file=config1.txt --from-file=config2.txt
```
- #### Delete command for already available configmap
```bash
>>>>>>> 436fddacc266561d953e85f84866907b4fec0758
     kubectl delete configmap <configmap_name>
```
    OR

```bash
     kubectl delete cm <configmap_name>
```

<<<<<<< HEAD

## K8s ConfigMap yml file to create configmap
Sample Template
=======
## 4) K8s ConfigMap yml file Sample Template
>>>>>>> 436fddacc266561d953e85f84866907b4fec0758

```Yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: <give_the_name_for_configmap>
data:
  key4: val4
  key2: val2
  key3: val3
```
## Pod yml file with ConfigMap to create pod.
Sample Template 

```Yaml
apiVersion: v1
kind: Pod
metadata:
  name: <give_the_name_for_pod>
spec:
  containers:
  - name: <give_the_name_for_container>
    image: <give_image_name_for_container>   
    envFrom:
    - configMapRef:
        name: <enter_already_create-configmap_name>
```

## Detailed-Description-Of-Secrets
Secret is an object used to store sensitive information, such as passwords, OAuth tokens, SSH keys, or any other sensitive data. The purpose of using Secrets is to avoid putting this sensitive information directly in your Pod or Deployment configuration files, which could be exposed.


- ### Create a secret from literal values

```bash
     kubectl create secret generic <give_name_of_secret> --from-literal username=admin --from-literal password='P@ssw0rd'
```
    Verify the secret:

```bash
    kubectl get secrets <enter_name_of_secret> -o yaml
```
The output will be base64 encoded. To decode, you can use:

```bash
    echo -n “P@ssw0rd” | base64
```

- ### Create a Secret from a File

Create a file with your sensitive data

username.txt and add content inside the file as: -
```bash
admin
```
password.txt and add content inside the file as: -
```bash
admin@123
```

```bash
     kubectl create secret generic <give_name_of_secret> --from-file username=./username.txt --from-file password=./password.txt
```
    Verify the secret:

```bash
    kubectl get secrets <enter_name_of_secre> -o yaml
```
The output will be base64 encoded.

- ### Create a YAML file for the secret

```Yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-manifest-secret
type: Opaque
data:
  username: YWRtaW4=    # base64 encoded value of 'admin'
  password: UEBzc3cwcmQ=  # base64 encoded value of 'P@ssw0rd'
```

Apply the YAML file to create secret
```bash
kubectl apply -f my-manifest-secret.yaml
```

- ### Create a pod that uses the secret:
```Yaml
apiVersion: v1
kind: Pod
metadata:
  name: <give_name_for_pod>
spec:
  containers:
  - name: <give_name_for_container>
    image: <enter_image_name_for_container>
    env:
    - name: USERNAME
      valueFrom:
        secretKeyRef:
          name: <give_name_for_secret>
          key: <key_valuename_like_username>
    - name: PASSWORD
      valueFrom:
        secretKeyRef:
          name: <give_name_for_secret>
          key: <key_valuename_like_password>
```







## Detailed-Description-Of-Deployment
Deployment is a resource used to manage and maintain the desired state of pod during updation of pod. It provides Rolling deployments updates to Pods ensuring your application is running reliably duriing updates. Updates are exposed to an increasing percentage of users incrementally until fully released.

Sample Template for deployment

```Yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: <give_name_for_deployment>
  labels:
    app: web
spec:
  replicas: 10
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: <give_name_for_container>
        image: <enter_imagename>
```
Roll Back Deployment command. It will restore previously deployment. Previous deployment is saved in k8s.

```bash
kubectl rollout undo deployment <enter_deployment_name>
```