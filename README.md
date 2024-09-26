# Lab: Understanding ConfigMaps, DaemonSets, Kubernetes Services, Secrets & Persistent Volume Claims

**Estimated time needed**: *1 hour*

I recently undertook a lab where I built and deployed a JavaScript application to Kubernetes using Docker. Throughout this journey, I delved into ConfigMaps, DaemonSets, Kubernetes Services, Secrets, and explored Volumes & Persistent Volume Claims. I wanted to document my experience and the steps I followed, both as a personal reference and to help others who might be going through a similar learning process.

## Objectives

By the end of this lab, I achieved the following:

- Built and deployed a JavaScript application to Kubernetes.
- Understood and created a ConfigMap.
- Implemented a DaemonSet.
- Created and exposed a Kubernetes Service.
- Managed sensitive data using Secrets.
- Defined Volumes and Persistent Volume Claims for storage.

## Prerequisites

Before starting, I ensured I had the following:

- Basic knowledge of Docker and Kubernetes.
- Access to a Kubernetes cluster.
- Docker installed on my local machine.
- The `kubectl` command-line tool configured to interact with my cluster.

## Getting Started

### Cloning the Repository

I began by cloning the repository containing the starter code:

```bash
git clone https://github.com/ibm-developer-skills-network/containers-project.git
cd containers-project/
```

### Setting Up My Namespace

I exported my namespace for consistent use throughout the lab:

```bash
export MY_NAMESPACE=sn-labs-$USERNAME
```

> **Note:** Replace `$USERNAME` with your actual username.

## Building and Deploying the Application

### Building the Docker Image

I built the Docker image and tagged it appropriately:

```bash
docker build . -t us.icr.io/$MY_NAMESPACE/myapp:v1
```

### Pushing the Image to the Container Registry

Next, I pushed the Docker image to the IBM Cloud container registry:

```bash
docker push us.icr.io/$MY_NAMESPACE/myapp:v1
```

### Verifying the Image

To confirm that the image was pushed successfully, I listed all images in my container registry:

```bash
ibmcloud cr images
```

I saw `myapp:v1` listed, confirming the push was successful.

### Deploying to Kubernetes

1. **Updating the Deployment Configuration:**
   
   I opened the `deployment.yml` file and replaced `<your SN labs namespace>` with my actual namespace.

2. **Applying the Deployment:**

   ```bash
   kubectl apply -f deployment.yml
   ```

3. **Verifying the Pods:**

   ```bash
   kubectl get pods
   ```

   I confirmed that the pods were up and running.

### Accessing the Application

To access the application locally, I set up port forwarding:

```bash
kubectl port-forward deployment.apps/myapp 3000:3000
```

I then navigated to `http://localhost:3000` in my browser and saw the application running, displaying the message:

```
Hello from MyApp. Your app is up!
```

**Note:** I pressed `CTRL + C` in the terminal to stop port forwarding when I was done.

## Exercises

### Exercise 1: ConfigMap

I learned how to create a ConfigMap to manage configuration data for the application.

- **Creating the ConfigMap:**

  ```bash
  kubectl create configmap myapp-config --from-literal=server-url=http://example.com --from-literal=timeout=5000
  ```

- **Verifying the ConfigMap:**

  ```bash
  kubectl get configmap myapp-config
  ```

  This showed that the ConfigMap was created successfully with the specified data.

### Exercise 2: DaemonSets

I explored how to implement a DaemonSet to ensure a pod runs on each node in the cluster.

- **Applying the DaemonSet Configuration:**

  ```bash
  kubectl apply -f daemonset.yaml
  ```

- **Verifying the DaemonSet:**

  ```bash
  kubectl get daemonsets
  ```

  The output confirmed that the DaemonSet was running as expected across all nodes.

### Exercise 3: Kubernetes Services

I created a Kubernetes Service to expose my application within the cluster.

- **Applying the Service Configuration:**

  ```bash
  kubectl apply -f service.yaml
  ```

- **Verifying the Service:**

  ```bash
  kubectl get services
  ```

  The service `myapp-service` was listed, indicating it was set up correctly.

### Exercise 4: Secrets

To securely handle sensitive information, I created a Secret.

- **Creating the Secret:**

  ```bash
  kubectl create secret generic myapp-secret --from-literal=username=myuser --from-literal=password=mysecretpassword
  ```

- **Verifying the Secret:**

  ```bash
  kubectl get secret myapp-secret
  ```

  The secret was created with the data entries, ready to be used in the application.

### Exercise 5: Volumes and Persistent Volume Claims

I defined Volumes and Persistent Volume Claims to provide persistent storage for my application.

- **Applying the Volume and PVC Configuration:**

  ```bash
  kubectl apply -f volume-and-pvc.yaml
  ```

- **Verifying the Persistent Volume and Claim:**

  ```bash
  kubectl get pv
  kubectl get pvc
  ```

  Both the PV and PVC were listed, showing they were set up correctly.

## Conclusion

Completing this lab was a rewarding experience. I deepened my understanding of Kubernetes by working hands-on with essential components like ConfigMaps, DaemonSets, Services, Secrets, and Persistent Volumes. This practical approach helped solidify concepts that are crucial for managing applications in a Kubernetes environment.

By building and deploying the JavaScript application, I saw firsthand how these components come together to create a robust, scalable, and secure application deployment. Managing configuration data, ensuring high availability, exposing services, handling sensitive information securely, and providing persistent storage are all critical aspects that I now feel more confident about.

---

**Author:** K Sundararajan

© IBM Corporation. All rights reserved.
