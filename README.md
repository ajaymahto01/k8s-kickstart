# k8s-kickstart
k8s-kickstart on simple minikube over docker

### Repository for the K8s in 1 hour video

#### K8s manifest files 
* mongo-config.yaml
* mongo-secret.yaml
* mongo.yaml
* webapp.yaml

#### K8s commands

##### start Minikube and check status
    minikube start --driver=docker 
    minikube status

##### get minikube node's ip address
    minikube ip

##### get basic info about k8s components
    kubectl get node
    kubectl get pod
    kubectl get svc
    kubectl get all

##### get extended info about components
    kubectl get pod -o wide
    kubectl get node -o wide

##### get detailed info about a specific component
    kubectl describe svc {svc-name}
    kubectl describe pod {pod-name}

##### get application logs
    kubectl logs {pod-name}

##### advanced logging and debugging commands

**Basic log retrieval:**
```bash
# Get logs from a specific pod
kubectl logs webapp-deployment-xyz123

# Get logs from a specific container in a multi-container pod
kubectl logs webapp-deployment-xyz123 -c webapp-container

# Get logs from all containers in a pod
kubectl logs webapp-deployment-xyz123 --all-containers=true
```

**Real-time and historical logs:**
```bash
# Follow logs in real-time (like tail -f)
kubectl logs -f webapp-deployment-xyz123

# Get last 50 lines of logs
kubectl logs webapp-deployment-xyz123 --tail=50

# Get logs from last 1 hour
kubectl logs webapp-deployment-xyz123 --since=1h

# Get logs with timestamps
kubectl logs webapp-deployment-xyz123 --timestamps=true
```

**Deployment and service logs:**
```bash
# Get logs from all pods in a deployment
kubectl logs deployment/webapp-deployment

# Get logs from all pods with a specific label
kubectl logs -l app=webapp

# Get previous container logs (useful if pod restarted)
kubectl logs webapp-deployment-xyz123 --previous
```

**Troubleshooting examples:**
```bash
# Check events for debugging issues
kubectl get events --sort-by=.metadata.creationTimestamp

# Describe pod for detailed status and events
kubectl describe pod webapp-deployment-xyz123

# Check resource usage
kubectl top pods
kubectl top nodes

# Get pod status and restart counts
kubectl get pods -o wide

# Debug networking issues
kubectl get svc
kubectl get endpoints
```
    
##### stop your Minikube cluster
    minikube stop

<br />

> :warning: **Known issue - Minikube IP not accessible** 

If you can't access the NodePort service webapp with `MinikubeIP:NodePort`, execute the following command:
    
    minikube service webapp-service

##### Alternative methods to access services in Minikube

**Method 1: Get direct URL to service**
```bash
minikube service <SERVICE_NAME> --url
```
This command will give you a direct URL to access the application. Copy the URL and access it in your web browser.

**Method 2: Port forwarding**
```bash
kubectl port-forward svc/<SERVICE_NAME> 3000:3000
```
After running this command, you can access the application on `localhost:3000` in your web browser.

**Example for webapp-service:**
```bash
# Method 1
minikube service webapp-service --url

# Method 2  
kubectl port-forward svc/webapp-service 3000:3000
```

> **Source:** [Stack Overflow - Unable to access Minikube IP address](https://stackoverflow.com/questions/71536310/unable-to-access-minikube-ip-address)

<br />

#### Links
* mongodb image on Docker Hub: https://hub.docker.com/_/mongo
* webapp image on Docker Hub: https://hub.docker.com/repository/docker/nanajanashia/k8s-demo-app
* k8s official documentation: https://kubernetes.io/docs/home/
* webapp code repo: https://gitlab.com/nanuchi/developing-with-docker/-/tree/feature/k8s-in-hour
