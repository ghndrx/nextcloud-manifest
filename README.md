# Nextcloud on Kubernetes

This repository contains a set of YAML files that can be used to deploy Nextcloud, MySQL, and an nginx ingress controller on a Kubernetes cluster.

## Prerequisites

- A Kubernetes cluster
- kubectl command-line tool installed and configured to access the cluster
- Docker installed on your machine
- A domain name that you can use to access the Nextcloud app and an SSL certificate

## Deployment

1. Build the Nextcloud Docker image and push it to a container registry, such as Docker Hub.
2. Create a new namespace for the deployment:
```
kubectl create namespace nextcloud
```
Create a Secret that contains the database credentials:

```
kubectl apply -f nextcloud-secret.yaml -n nextcloud
```
Create a ConfigMap that contains the nginx ingress controller configuration:
```
kubectl apply -f nginx-ingress-configmap.yaml -n nextcloud
```
Create a Secret that contains the SSL certificate and key:

```
kubectl apply -f nginx-ingress-secret.yaml -n nextcloud
```
Create a Persistent Volume Claim:
```
kubectl apply -f nextcloud-pvc.yaml -n nextcloud
```
Create the MySQL deployment:
```
kubectl apply -f mysql-deployment.yaml -n nextcloud
```
Create the Nextcloud deployment:
```
kubectl apply -f nextcloud-deployment.yaml -n nextcloud
```
Create the services:
```
kubectl apply -f nextcloud-service.yaml -n nextcloud
kubectl apply -f mysql-service.yaml -n nextcloud
```
Create the ingress resources
```
kubectl apply -f nextcloud-ingress.yaml -n nextcloud
```

Accessing Nextcloud
Once the deployment is complete, you should be able to access the Nextcloud app using the domain name that you have configured in the ingress resource.

Cleanup
To remove the Nextcloud deployment from the cluster, use the following command:

```
kubectl delete -f . -n nextcloud
```
Troubleshooting
If you encounter any issues with the deployment, you can use the following command to view the logs for the Nextcloud and MySQL pods:
```
kubectl logs -f <pod-name> -n nextcloud
```
You can also check the events and status of the resources using

```
kubectl describe <resource-type> <resource-name> -n nextcloud
```

It is important to note that you will need to replace the placeholder values in the commands above with actual values that are specific to your deployment.

Also, make sure that the version of the services and images are compatible with your kubernetes version.

It is a good practice to check the official documentation for each of the services and tools used in this deployment to ensure that you have the latest information on configuration options and troubleshooting steps.

Additionally, you can use the Kubernetes dashboard or other monitoring tools to view the status of the pods, services, and other resources in your cluster.

It is also recommended to test the deployment in a development or staging environment before deploying to a production cluster.

Finally, make sure you have backups of your data before doing any major changes.