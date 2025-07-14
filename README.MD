## Capstone Project 4: Microservices deployment on LKE

### What did I build?
I built a Kubernetes-based deployment environment on Linode using the Linode CLI and deployed the Online Boutique microservices application into this cluster. The cluster was provisioned with the linode-cli, and I connected to it via kubectl using the generated kubeconfig. The application was deployed using a complete Kubernetes manifest file.

### What have I learned?
Through this project, I gained hands-on experience with:
Installing and configuring the Linode CLI.
Creating and managing a Kubernetes cluster on Linode.
Exporting and using the kubeconfig file to interact with the cluster.
Deploying a multi-service application using Kubernetes manifests.
Understanding how microservices interact in a production-like environment.
This improved my understanding of cloud-native deployment, Kubernetes resource management, and using CLI tools to manage infrastructure.

### My project description. 



# CAPSTONE PROJECT-4 project execution


## Step 1: Installing Linode CLI

First, I installed the Linode CLI using Homebrew:

```sh
brew install linode-cli
```

> ✅ Homebrew auto-updated. Linode CLI was installed successfully.

---

## Step 2: Configuring Linode CLI

Next, I configured the Linode CLI to authenticate and use my account:

```sh
linode-cli configure
```

You’ll be prompted to enter your personal access token and default settings like region and output format.

---

## Step 3: Creating a Kubernetes Cluster on Linode

Initially, I attempted to create a cluster using Kubernetes version `1.28`, which failed:

```sh
linode-cli lke cluster-create \
  --label my-linode-cluster \
  --region eu-west \
  --k8s_version 1.28 \
  --node_pools.type g6-standard-2 \
  --node_pools.count 2
```

**Error:**

```
Request failed: 400
errors                                    
┌─────────────┬──────────────────────────┐
│ field       │ reason                   │
├─────────────┼──────────────────────────┤
│ k8s_version │ k8s_version is not valid │
└─────────────┴──────────────────────────┘
```

To fix this, I listed the available Kubernetes versions:

```sh
linode-cli lke versions-list
```

**Available versions:**

```
1.33
1.32
1.31
```

Then I successfully created the cluster with version `1.31` in the `eu-central` region:

```sh
linode-cli lke cluster-create \
  --label my-linode-cluster \
  --region eu-central \
  --k8s_version 1.31 \
  --node_pools.type g6-standard-2 \
  --node_pools.count 2
```

**Output:**

```
┌────────┬───────────────────┬────────────┬─────────────┬─────────────────────────────────┬──────┐
│ id     │ label             │ region     │ k8s_version │ control_plane.high_availability │ tier │
├────────┼───────────────────┼────────────┼─────────────┼─────────────────────────────────┼──────┤
│ 497779 │ my-linode-cluster │ eu-central │ 1.31        │ False                           │      │
└────────┴───────────────────┴────────────┴─────────────┴─────────────────────────────────┴──────┘
```
![Cluster Info-1](./Bildschirmfoto%202025-07-11%20um%2017.34.03.png)
![Cluster Info-2](./Bildschirmfoto%202025-07-11%20um%2017.34.08.png)
![Cluster Info-3](./Bildschirmfoto%202025-07-11%20um%2017.34.38.png)

---

## Step 4: Connecting to the Cluster

I exported the Kubeconfig to interact with the cluster via `kubectl`:

```sh
export KUBECONFIG=my-linode-cluster-kubeconfig.yaml
```

After that, I verified the cluster was up and running:

```sh
sgworker@MacBook-Pro-3.local /Users/sgworker/Desktop/CAPSTONE-PROJECT-4 
% kubectl get nodes                           
NAME                            STATUS   ROLES    AGE   VERSION
lke497779-717665-25ab23ea0000   Ready    <none>   12m   v1.31.0
lke497779-717665-281796a70000   Ready    <none>   12m   v1.31.0


```

---

## Step 5: Deploying the Application

I deployed the microservices application (Online Boutique) using a Kubernetes manifest:

```sh
sgworker@MacBook-Pro-3.local /Users/sgworker/Desktop/CAPSTONE-PROJECT-4 
% kubectl apply -f linode-boutique/kubernetes-manifests.yaml 
deployment.apps/emailservice created
service/emailservice created
serviceaccount/emailservice created
deployment.apps/checkoutservice created
service/checkoutservice created
serviceaccount/checkoutservice created
deployment.apps/recommendationservice created
service/recommendationservice created
serviceaccount/recommendationservice created
deployment.apps/frontend created
service/frontend created
service/frontend-external created
serviceaccount/frontend created
deployment.apps/paymentservice created
service/paymentservice created
serviceaccount/paymentservice created
deployment.apps/productcatalogservice created
service/productcatalogservice created
serviceaccount/productcatalogservice created
deployment.apps/cartservice created
service/cartservice created
serviceaccount/cartservice created
deployment.apps/redis-cart created
service/redis-cart created
deployment.apps/loadgenerator created
serviceaccount/loadgenerator created
deployment.apps/currencyservice created
service/currencyservice created
serviceaccount/currencyservice created
deployment.apps/shippingservice created
service/shippingservice created
serviceaccount/shippingservice created
deployment.apps/adservice created
service/adservice created
serviceaccount/adservice created

```

This created all deployments and services for the microservices shop.

---

## Step 6: Accessing the Shop

I located the external IP from the `frontend-external` service:

```sh
sgworker@MacBook-Pro-3.local /Users/sgworker/Desktop/CAPSTONE-PROJECT-4 
% kubectl get svc frontend-external

NAME                TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)        AGE
frontend-external   LoadBalancer   10.128.125.46   194.195.247.51   80:31869/TCP   70s
sgworker@MacBook-Pro-3.local /Users/sgworker/Desktop/CAPSTONE-PROJECT-4 
% kubectl get svc frontend-external -o yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"Service","metadata":{"annotations":{},"labels":{"app":"frontend"},"name":"frontend-external","namespace":"default"},"spec":{"ports":[{"name":"http","port":80,"targetPort":8080}],"selector":{"app":"frontend"},"type":"LoadBalancer"}}
  creationTimestamp: "2025-07-11T15:49:06Z"
  finalizers:
  - service.kubernetes.io/load-balancer-cleanup
  labels:
    app: frontend
  name: frontend-external
  namespace: default
  resourceVersion: "1729"
  uid: a22e896b-0c8e-4d9e-9d98-fece57b42e57
spec:
  allocateLoadBalancerNodePorts: true
  clusterIP: 10.128.125.46
  clusterIPs:
  - 10.128.125.46
  externalTrafficPolicy: Cluster
  internalTrafficPolicy: Cluster
  ipFamilies:
  - IPv4
  ipFamilyPolicy: SingleStack
  ports:
  - name: http
    nodePort: 31869
    port: 80
    protocol: TCP
    targetPort: 8080
  selector:
    app: frontend
  sessionAffinity: None
  type: LoadBalancer
status:
  loadBalancer:
    ingress:
    - hostname: 194-195-247-51.ip.linodeusercontent.com
      ip: 194.195.247.51
      ipMode: VIP
```

Using the EXTERNAL-IP, I accessed the application in the browser.
![Browser Info-1](./Bildschirmfoto%202025-07-11%20um%2017.52.32.png)
![Browser Info-2](./Bildschirmfoto%202025-07-11%20um%2017.52.41.png)
![Browser Info-3](./Bildschirmfoto%202025-07-11%20um%2017.52.54.png)



---

## Step 7: Cleaning Up

To avoid ongoing charges, I deleted the Linode cluster using the CLI:

```sh
linode-cli lke cluster-delete 497779
```

---

## What Did I Build?

I built a Kubernetes-based microservices application on Linode using:

- **Linode CLI** to provision the cluster
- **kubectl** to manage the deployments
- **YAML manifests** to deploy Google’s Online Boutique
- **LoadBalancer service** to expose the frontend to the internet

---

## What Did I Learn?

- How to use Linode CLI to provision infrastructure
- Working with Kubernetes clusters on Linode
- Deploying and managing microservices with Kubernetes
- Connecting clusters using `kubectl` and Kubeconfig
- How to expose services using a LoadBalancer

---

## Additional Notes

- Region used: `eu-central`
- Kubernetes version: `1.31`
- Node pool: 2 × `g6-standard-2`
- App used: **Online Boutique** (Google Microservices Demo)
