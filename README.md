# Deployment of a Reddit Clone App on Kubernetes with Ingress

![Untitled Diagram](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/a20703b8-335d-432c-a244-06905ff2435b)


## Prerequisites

1) EC2 - t2.micro and t2.medium
2) Git
3) Docker 
4) Minikube 
5) kubectl

1. Install Git and Docker on EC2 t2.micro
2. Install Docker, Minikube and kubectl on EC2 t2.medium

## Step-1

### Launch EC2 instance t2.micro

![Screenshot 2023-10-03 170602](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/c2c18ea8-b16a-4899-93f4-311f78eb3dd9)

Install docker : https://docs.docker.com/engine/install/ubuntu/

Add user to docker group 

```
usermod -aG docker #USER
reboot
```

### Clone Github repository 

```
https://github.com/pradip2994/Reddit-kubernetes-Project.git
```
Go inside directory and build docker image 

```
cd Reddit-kubernetes-Project

docker build -t pradipkv/reddit_app .
```

![image](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/93f8708f-9a31-4300-ab38-506c24cef019)

Now you can see that Images has been created

![image](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/c583fb32-63fe-4aa3-b34d-108af2251ab2)

Now Login to docker hub 

![image](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/8ca61e27-699a-46ad-bb4d-666b9dd13d2d)

Now push docker image to DockerHub

```

docker push pradipkv/reddit_app
```

![image](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/b905db7a-b65f-4ce9-98db-1d7d86c03bda)

Now you can see that image has been pushed to DockerHub

![image](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/e123b78e-ab00-483b-a9d1-7a99a1bbd7c2)

## Step-2

### Launch EC2 instance t2.medium

![image](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/d46deeb5-9042-43b0-b16a-7c2d47ace1e7)

Install docker : https://docs.docker.com/engine/install/ubuntu/

Add user to docker group 

```
usermod -aG docker #USER
reboot
```
Install Minikube : https://minikube.sigs.k8s.io/docs/start/

```
minikube start --driver=docker
```

![image](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/227116c6-7666-4d61-bf8e-0ec6e119f29b)

Once minikube start finishes, run the command below to check the status of the cluster:

```
$minikube status
```
Install kubectl

```
$sudo snap install kubectl --classic
$kubectl version
```

![image](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/b165ea0f-edca-4ab1-879f-6c46595b4b20)

### Create YAML Manifest

```
vim reddit-deployment.yaml
vim redit-service.yaml
vim reddit-ingress.yaml
```

The bellow command enable the Ingress addon in a Minikube Kubernetes cluster. This addon allows you to use Ingress resources to expose services and routes HTTP traffic into your Minikube cluster.

```
minikube addons enable ingress
```

![image](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/fcae6347-37c1-49b4-9a24-cc4e20b83491)

The command minikube addons list is used to list the available addons and their status in your Minikube Kubernetes cluster. This command provides information about which addons are currently enabled and which ones are disabled.

```
minikube addons list
```

![image](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/ebd35e00-4d8e-403c-bbbb-095f6b64dc8b)

The below commands apply the configurations defined in the YAML files to your Kubernetes cluster.

```
kubectl apply -f reddit-deployment.yaml
kubectl apply -f redit-service.yaml
kubectl apply -f reddit-ingress.yaml
```

![image](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/451273d6-7aed-452b-b3bf-a0e65f7001c2)

When you run these commands, you'll get output displaying the relevant information for each resource type.

```
$kubectl get pods
$kubectl get deploy
$kubectl get service
$kubectl get ingress

```

![image](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/2e0aadc7-5352-4e8c-9b30-5533114cff69)

Add port range in Security Group 

![image](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/6b5a2ba9-6878-40a8-b5ec-f895f10aaf7b)

### To expose application to the internet use command
```
kubectl port-forward svc/reddit-app-service 3000:3000 --address 0.0.0.0
```

![image](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/389508cd-336f-489e-bcf5-7f54994813b6)

To access application enter <ec2_instance_IP>:<port_number> in your browser.
```
13.127.42.107:3000
```
![image](https://github.com/pradip2994/Reddit-kubernetes-Project/assets/124191442/542250a1-dcbb-4239-bcc9-12e6e6817203)
