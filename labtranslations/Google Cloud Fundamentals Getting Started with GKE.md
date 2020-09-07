# LAB:Google Cloud Fundamentals: Getting Started with GKE
----


## Objectives:
- In this lab, you learn how to perform the following tasks:

- Provision a Kubernetes cluster using Kubernetes Engine.

- Deploy and manage Docker containers using kubectl.

## Steps
1. Confirm that needed APIs are enabled
In the GCP Console, on the Navigation menu, click APIs & Services.
Scroll down in the list of enabled APIs, and confirm that both of these APIs are enabled:
Kubernetes Engine API
Container Registry API
If either API is missing, click Enable APIs and Services at the top. Search for the above APIs by name and enable each for your current project.
-------
2. Start a Kubernetes Engine cluster
For convenience, place the zone that Qwiklabs assigned you to into an environment variable called MY_ZONE. At the Cloud Shell prompt, type this partial command:
```
export MY_ZONE=
```
followed by the zone that Qwiklabs assigned to you. Your complete command will look similar to this:
```
export MY_ZONE=us-central1-a
```
- Start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster webfrontend and configure it to run 2 nodes:
```
gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2
```
- After the cluster is created, check your installed version of Kubernetes using the kubectl version command:
```
kubectl version
```
-------
3. Run and deploy a container
- From your Cloud Shell prompt, launch a single instance of the nginx container. (Nginx is a popular web server.)
```
kubectl create deploy nginx --image=nginx:1.17.10
```
- View the pod running the nginx container:
```
kubectl get pods
```
- Expose the nginx container to the Internet:
```
kubectl expose deployment nginx --port 80 --type LoadBalancer
```
- Kubernetes created a service and an external load balancer with a public IP address attached to it. The IP address remains the same for the life of the service. Any network traffic to that public IP address is routed to pods behind the service: in this case, the nginx pod.
- View the new service:
```
kubectl get services
```
- Scale up the number of pods running on your service:
```
kubectl scale deployment nginx --replicas 3
```
- Scaling up a deployment is useful when you want to increase available resources for an application that is becoming more popular.
- Confirm that Kubernetes has updated the number of pods:
```
kubectl get pods
```
Return to the web browser tab in which you viewed your cluster's external IP address. Refresh the page to confirm that the nginx web server is still responding.