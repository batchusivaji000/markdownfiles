## Resources

#### Kubernetes Docs
https://kubernetes.io/docs/home/

#### Kubectl Cheatsheet
https://kubernetes.io/docs/reference/kubectl/cheatsheet/

<h1></h1>

## Kubernetes Commands:
#### To see the nodes:
```
kubectl get nodes
```

#### To see the cluster info:
```
kubectl cluster-info
```

#### To run a docker container on a pod:
```
kubectl run kubetest —image=nginx:latest —port=80
```

#### To see running pods:
```
kubectl get pods
```

#### To see detailed pod info:
```
kubectl get pods -o wide
```

#### To see all info about running pods:
```
kubectl describe pods
```

#### To delete pods:
```
kubectl delete pods mypod-name
```

#### To apply a kubernetes deployment file:
```
kubectl apply -f mydeplyment.yaml
```

#### To check running deployments:
```
kubectl get deployments 
```

#### To increase number of replicas running the deployment:
Edit the running deployment with:
```
kubectl edit deployment mydeployment
```

Change number of “replicas” on that config and save the file and check pods again:
```
kubectl get pods (number of running pods should change)
```

#### To see detailed info about the services:
```
kubectl describe services mywebsite_service
```

#### To apply a kubernetes deployment file:
```
kubectl apply -f mydeplyment.yaml
```

#### To check running deployments:
```
kubectl get deployments
```

#### To edit the running deployment:
```
kubectl edit deployment mydeployment
```

#### To scale replicas of a deployment:
```
kubectl scale --replicas=3 deployment web-app
```

#### To restart a pod:
```
kubectl scale deployment traefik --replicas=0 -n traefik
kubectl scale deployment traefik --replicas=1 -n traefik
```

#### To create a deployment:
```
kubectl create deployment nginx --image=nginx
```

#### To expose a service:
```
kubectl expose deployment nginx --type=NodePort --port=8080
```

#### To check services:
```
kubectl get services
```

#### To see all infos about current namespace:
```
kubectl get all
```

#### To see all activity:
```
kubectl get events
```

#### To see cpu and memory usage of pods from all namespaces:
```
kubectl top pods -A
```

#### To see all namespaces:
```
kubectl get namespaces
or,
kubectl get ns
```

#### To create namespace:
```
kubectl create namespace test
```

#### To set the namespace for a current request, use the --namespace flag:
```
kubectl run nginx --image=nginx --namespace=<insert-namespace-name-here>
kubectl get pods --namespace=<insert-namespace-name-here>
```

#### To change to another namespace:
```
kubectl config set-context --current --namespace=<insert-namespace-name-here>
or,
kn test (if zsh is configured for that)
```

#### To go back to default namespace:
```
knd (if zsh is configured for that)
```

#### To delete a namespace:
```
kubectl delete ns test
```

#### To delete multiple nodes at once (eg. wordpress):
```
kubectl get pods -n default | awk '/wordpress/{print $1}' | xargs kubectl delete -n default pod
or,
kubectl get pods | awk '/wordpress/{print $1}' | xargs kubectl delete pod
```

#### To create sample deployment yaml from command:
```
kubectl create deploy nginx --image nginx --dry-run=client -o yaml > deployment.yaml
```

#### To enable Horizontal Pod Autoscale (HPA) for a deployment eg. nginx:
```
kubectl autoscale deploy nginx --min 1 --max 5 --cpu-percent 20
```

#### To test autoscale use siege to simulate traffic:
```
sudo apt install siege -y
sample command:
siege -q -c 5 -t 2m http://80.0.0.103
```

#### To check live pod cpu utilizations by:
```
watch kubectl top pods
```

#### To check live HPA updates and pod creations:
```
watch kubectl get all
```

#### To check secrets:
```
kubectl get secrets
```

#### To decode secrets:
```
kubectl get secret db-user-pass -o jsonpath='{.data}'
kubectl get secret local-example-com-tls -o jsonpath='{.data}'  | awk -F: '{ print $2 }' | tr -d ',""' | sed 's/tls.key//' | base64 --decode > /tmp/tls.crt
kubectl get secret local-example-com-tls -o jsonpath='{.data}'  | awk -F: '{ print $3 }' | tr -d '"}' | base64 --decode > /tmp/tls.key
```

#### To get container image versions in all namespaces:
```
kubectl get pods --all-namespaces -o jsonpath="{.items[*].spec.containers[*].image}" |\
tr -s '[[:space:]]' '\n' |\
sort |\
uniq -c
```

#### To update helm repo:
```
helm repo update
```

#### To upgrade applications installed using helm:
```
helm upgrade -n portainer portainer portainer/portainer
```

#### To delete a pod stuck in terminating status forcefully:
```
kubectl delete pod  traefik-f57964d5f-tvh9q --grace-period=0 --force -n kube-system
```

#### To get replicaset:
```
kubectl get replicaset
or,
kubectl get rs
```

#### To get endpoints:
```
kubectl get endpoints 
or,
kubectl get ep
```

#### To get explanation of anything:
```
kubectl explain deployment 
```
Or to see explanation of any specific section of a resource:
```
kubectl explain deployment.spec.template.spec.nodeName
```

#### To see all resource definitions:
```
kubectl api-resources
kubectl api-resources | grep -i net
```

#### To see network policies:
```
kubectl get netpol,cnp,ccnp
ccp - ciliumnetworkpolicy
ccnp - ciliumclusterwidenetworkpolicy
kubectl get netpol allow-egress -o yaml
```

#### To check cilium implementation and understand network policies:
```
https://editor.cilium.io
```

#### To extend verbosity level:
```
kubectl get pods -A -v=9
```

#### To wait or watch updating outputs of any kubectl command:
```
kubectl get pods -w
```

#### For etcd related issues:
Google etcd cheatsheet
```
apt install etcd-client
etcdctl endpoint status
etcdctl alarm list
```

#### To replace a running pod with pod definition:
```
kubectl replace --force -f /tmp/kubectl-edit-28112.yaml
```

## Imperative Commands:
#### Create an NGINX Pod:
```
kubectl run nginx --image=nginx
```

#### Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run):
```
kubectl run nginx --image=nginx --dry-run=client -o yaml
```

#### Create a deployment:
```
kubectl create deployment nginx --image=nginx
```

#### Generate Deployment YAML file (-o yaml). Don't create it(--dry-run):
```
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml
```

#### Generate Deployment YAML file (-o yaml). Don't create it(--dry-run) with 4 Replicas (--replicas=4):
```
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml
```

Save it to a file, make necessary changes to the file (for example, adding more replicas) and then create the deployment:
```
kubectl create -f nginx-deployment.yaml
```

Or,

In k8s version 1.19+, we can specify the --replicas option to create a deployment with 4 replicas:

```
kubectl create deployment nginx --image=nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
```

#### Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379:
```
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml
```

This will automatically use the pod's labels as selectors

Or,

```
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml 
```

This will not use the pods labels as selectors, instead it will assume selectors as app=redis. You cannot pass in selectors as an option. So it does not work very well if your pod has a different label set. So generate the file and modify the selectors before creating the service

#### Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes:
```
kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml
```

This will automatically use the pod's labels as selectors, but you cannot specify the node port. You have to generate a definition file and then add the node port in manually before creating the service with the pod.

Or,

```
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
```

This will not use the pods labels as selectors

Both the above commands have their own challenges. While one of it cannot accept a selector the other cannot accept a node port. I would recommend going with the kubectl expose command. If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.

#### To manually schedule a pod on a specific node:
Add the **`nodeName`** section below spec->containers section on pod definition  yaml--
```
apiVersion: v1
kind: Pod
metadata:
 name: nginx
 labels:
  name: nginx
spec:
 containers:
 - name: nginx
   image: nginx
   ports:
   - containerPort: 8080
 nodeName: node02
```

Another way to manually assign node without scheduler using manifest:
```
apiVersion: v1
kind: Binding
metadata:
  name: nginx
target:
  apiVersion: v1
  kind: Node
  name: node02
```

Then use the **`curl`** command to initiate a **`POST`** request
```
curl --header "Content-Type:application/json" --request POST --data  '{"apiVersion":"v1", "kind":"Binding"...}' http://$SERVER/api/v1/default/pods/$PODNAME/binding/
```

#### To add label selector to pod definition:
```
 apiVersion: v1
 kind: Pod
 metadata:
  name: simple-webapp
  labels:
    env: dev
    bu: finance
    tier: frontend
 spec:
  containers:
  - name: simple-webapp
    image: simple-webapp
    ports:
    - containerPort: 8080
```

#### To add label selector to deployment definition:
```
 apiVersion: apps/v1
 kind: ReplicaSet
 metadata:
   name: simple-webapp
   labels:
     env: dev
     bu: finance
     tier: frontend
 spec:
  replicas: 3
  selector:
    matchLabels:
     env: dev
     bu: finance
     tier: frontend
 template:
   metadata:
     labels:
       env: dev
       bu: finance
       tier: frontend
   spec:
     containers:
     - name: simple-webapp
       image: simple-webapp   
```

#### To add label selector to service definition:
```
  apiVersion: v1
  kind: Service
  metadata:
   name: my-service
  spec:
   selector:
     env: dev
     bu: finance
     tier: frontend
   ports:
   - protocol: TCP
     port: 80
     targetPort: 9376 
```

#### To find pods using selectors:
```
kubectl get pods --selector env=dev
kubectl get all --selector env=prod,bu=finance,tier=frontend
```

#### To add taint to a node:
```
kubectl taint nodes <node-name> key=value:taint-effect
kubectl taint node node01 app=blue:NoSchedule
```
There are 3 taint effects

- NoSchedule
- PreferNoSchedule
- NoExecute

#### To add toleration to a pod:
Add the **`tolerations`** section below spec->containers section inside pod definition  yaml:

```
apiVersion: v1
kind: Pod
metadata:
 name: myapp-pod
spec:
 containers:
 - name: nginx-container
   image: nginx
 tolerations:
 - key: "app"
   operator: "Equal"
   value: "blue"
   effect: "NoSchedule"
```

#### To label nodes:
```
kubectl label nodes <node-name> <label-key>=<label-value>
kubectl label nodes node-1 size=Large
```

Then add the **`nodeSelector`** section below spec->containers section inside pod definition yaml:
```
apiVersion: v1
kind: Pod
metadata:
 name: myapp-pod
spec:
 containers:
 - name: data-processor
   image: data-processor
 nodeSelector:
  size: Large
```

#### Affinity:
With Node Selectors we cannot provide the advance expressions.
The primary feature of Node Affinity is to ensure that the pods are hosted on particular nodes.

Add the **`Affinity`** section below spec->containers section inside pod definition yaml:
```
apiVersion: v1
kind: Pod
metadata:
 name: myapp-pod
spec:
 containers:
 - name: data-processor
   image: data-processor
 affinity:
   nodeAffinity:
     requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In
            values: 
            - Large
            - Medium
```
or,
```
apiVersion: v1
kind: Pod
metadata:
 name: myapp-pod
spec:
 containers:
 - name: data-processor
   image: data-processor
 affinity:
   nodeAffinity:
     requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: NotIn
            values: 
            - Small
```
or,
```
apiVersion: v1
kind: Pod
metadata:
 name: myapp-pod
spec:
 containers:
 - name: data-processor
   image: data-processor
 affinity:
   nodeAffinity:
     requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: Exists
```

#### Node Affinity Types
```
requiredDuringSchedulingIgnoredDuringExecution
preferredDuringSchedulingIgnoredDuringExecution
```

#### To create a Static Pod on a Node:
```
kubectl run static-busybox --image=busybox --restart=Never --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/busybox.yaml
```
Static Pods must be yaml files placed inside node directory /etc/kubernetes/manifests, and kubelet will create the Pod automatically.

#### To view kube-scheduler logs:
```
kubectl logs kube-scheduler-controlplane -n kube-system
```

#### To rollout and rollback a deployment
```
kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1
or, just change image version on deployment.yaml and "kubectl apply -f deployment.yaml"

kubectl rollout status deployment/myapp-deployment
kubectl rollout history deployment/myapp-deployment
kubectl rollout undo deployment/myapp-deployment
```

#### Commands and Arguments
```
apiVersion: v1
kind: Pod
metadata:
  name: command-demo
  labels:
    purpose: demonstrate-command
spec:
  containers:
  - name: command-demo-container
    image: ubuntu
    command: ["/bin/sh"]
    args: ["-c", "while true; do echo hello; sleep 10;done"]
  restartPolicy: OnFailure
```
"command" in Kubernetes is similar to Entrypoints in Docker, which is the default command to run when the pod is initialized.
"args" in Kubernetes is similar to CMD in Docker, which can be replaced during execution.

#### curl-test.sh -to test something out of a pod
```
for i in {1..35}; do
   kubectl exec --namespace=kube-public curl -- sh -c 'test=`wget -qO- -T 2  http://webapp-test/info 2>&1` && echo "$test OK" || echo "Failed"';
   echo ""
done
```
Image used for curl: byrnedo/alpine-curl:latest

#### To get help for any kubectl command
```
kubectl run --help
```

#### To add a command or argument while running a pod
```
kubectl run webapp-green --image=kodekloud/webapp-color -- --color=green
kubectl run webapp-green --image=kodekloud/webapp-color --command -- python app.py --color=green
```

#### To update a pod with changed values that is not possible with edit
Edit the pod with "kubectl edit pod podname".
When a temp file is saved with the edit, then replace the running pod with edited one.
```
kubectl replace --force -f /tmp/kubectl-edit-3321164344.yaml
```

#### To create a secret containing multiple key and values
```
kubectl create secret generic db-secret --from-literal=DB_Host=sql01 --from-literal=DB_User=root --from-literal=DB_Password=password123
```

#### To add secretRef and configMapRef on pod definition
```
containers:
  - image: kodekloud/simple-webapp-mysql
    imagePullPolicy: Always
    name: webapp
    envFrom:
    - secretRef:
        name: db-secret
```

```
containers:
  - image: kodekloud/webapp-color
    imagePullPolicy: Always
    name: webapp
    envFrom:
    - configMapRef:
        name: webapp-color-config
```

#### Init Containers
An initContainer is configured in a pod like all other containers, except that it is specified inside a initContainers section,  
like this:
```
containers:
- name: myapp-container
  image: busybox:1.28
  command: ['sh', '-c', 'echo The app is running! && sleep 3600']
initContainers:
- name: init-myservice
  image: busybox
  command: ['sh', '-c', 'git clone <some-repository-that-will-be-used-by-application> ; done;']
```
When a POD is first created the initContainer is run, and the process in the initContainer must run to a completion 
before the real container hosting the application starts. 

You can configure multiple such initContainers as well, like multi-pod containers. 
In that case each init container is run one at a time in sequential order.

If any of the initContainers fail to complete, Kubernetes restarts the Pod repeatedly until the Init Container succeeds.
```
containers:
- name: myapp-container
  image: busybox:1.28
  command: ['sh', '-c', 'echo The app is running! && sleep 3600']
initContainers:
- name: init-myservice
  image: busybox:1.28
  command: ['sh', '-c', 'until nslookup myservice; do echo waiting for myservice; sleep 2; done;']
- name: init-mydb
  image: busybox:1.28
  command: ['sh', '-c', 'until nslookup mydb; do echo waiting for mydb; sleep 2; done;']
```

#### To move running pods to different node for Node maintenance
```
kubectl drain node-1
or,
kubectl drain node-1 --ignore-daemonsets
or,
kubectl drain node-1 --ignore-daemonsets --force   # this will remove any running pod that is not part of a replicaset, etc.
```
This will gracefully terminate all running pods on the node and recreate them on a different node, 
and mark the node as SchedulingDisabled.

```
kubectl uncordon node-1
```
After the maintenance or reboot of the node, this command remove the SchedulingDisabled tag on the node,
and node will be ready again for scheduling. But pods moved to other nodes while draining will not be re-scheduled on this node automatically.
But new pods will start to schedule on this pod after uncordoning.

```
kubectl cordon node-2
```
This will mark the node as SchedulingDisabled, existing pods will keep running on same node,
but all new pods will not be scheduled on this node.


#### Upgrading Cluster (Using kubeadm)
Refer to the kubernetes.io/docs and search for "kubeadm upgrade".
To upgrade a cluster first the master node needs to be upgraded then the worker nodes.

On Master node:
```
kubectl get nodes
kubectl get pods -o wide
kubectl drain controlplane --ignore-daemonsets

apt update
apt-cache madison kubeadm

apt-mark unhold kubeadm && \
apt-get update && apt-get install -y kubeadm=1.24.0-00 && \
apt-mark hold kubeadm

sudo kubeadm upgrade plan
sudo kubeadm upgrade apply v1.24.0
 
apt-mark unhold kubelet kubectl && \
apt-get update && apt-get install -y kubelet=1.24.0-00 kubectl=1.24.0-00 && \
apt-mark hold kubelet kubectl

sudo systemctl daemon-reload
sudo systemctl restart kubelet

kubectl uncordon controlplane

kubectl drain node01 --ignore-daemonsets          # before starting upgrade on worker node
kubectl uncordon node01                           # after upgrade of worker node is finished
```

On Worker nodes:
```
apt update

apt-mark unhold kubeadm && \
apt-get update && apt-get install -y kubeadm=1.24.0-00 && \
apt-mark hold kubeadm

sudo kubeadm upgrade node

apt-mark unhold kubelet kubectl && \
apt-get update && apt-get install -y kubelet=1.24.0-00 kubectl=1.24.0-00 && \
apt-mark hold kubelet kubectl

sudo systemctl daemon-reload
sudo systemctl restart kubelet
```




---
## Minikube:

#### Install minikube on local machine:
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

#### Start a sample cluster:
```
minikube start
```

#### Check the pods of cluster:
```
kubectl get po -A
```

#### If minikube not installed:
```
minikube kubectl -- get po -A
```

#### If you need to make using minikube simpler:
```
alias kubectl="minikube kubectl --"
```

#### To check kubernetes dashboard:
```
minikube dashboard
```

#### To deploy kubernetes dashbaord outside minikube:
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.5.0/aio/deploy/recommended.yaml
```

You can enable access to the Dashboard using the kubectl command-line tool, by running the following command:
```
kubectl proxy
```
Kubectl will make Dashboard available at:
```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/.
```

#### To create a sample deployment and expose it on port 8080:
```
kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
kubectl expose deployment hello-minikube --type=NodePort --port=8080
```

#### To check deployment:
```
kubectl get services hello-minikube
```

#### To access this service let minikube launch a web browser for you:
```
minikube service hello-minikube
```

Alternatively, use kubectl to forward the port:
```
kubectl port-forward service/hello-minikube 7080:8080
```

---
#### To install HELM:

```
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

#### To add a helm repo:
```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

#### To install apps (eg. nginx) from helm repo:
```
helm install my-nginx-test bitnami/nginx
```

#### To uninstall/delete a helm chart/deployment:
```
helm delete my-release
```

#### To check the status of the service deployment:
```
kubectl get svc --namespace default -w my-nginx-test
or,
kubectl get services my-nginx-test
```

#### To expose a port for the latest deployment:
```
kubectl expose deployment my-nginx-test --type=NodePort --port=80
```

#### To forward the exposed port of the deployment to the localhost:
```
kubectl port-forward service/my-nginx-test 4000:80
```

#### To download and unpack helm package for an application:
```
helm fetch stable/traefik --untar
helm pull
helm init
```

---
#### Install portainer using helm:
```
helm repo add portainer https://portainer.github.io/k8s/
helm repo update
helm install --create-namespace -n portainer portainer portainer/portainer \
    --set service.type=LoadBalancer
```

Or, if using traefik then,
```
helm install --create-namespace -n portainer portainer portainer/portainer
```

Or. if using microk8s, then
```
microk8s.enable community
microk8s.enable portainer
```

And then apply the portainer-ing-svc-tls.yaml template from kubernetes yamls directory

#### To update portainer version:
```
helm repo update
helm upgrade -n portainer portainer portainer/portainer
```

Source:
https://docs.portainer.io/v/ce-2.9/start/install/server/kubernetes/wsl

---
#### Install wordpress using helm:
```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

Setting up wordpress directly from helm along with mariadb is always giving error.
So, best way is setting up mariadb and wordpress separately.

First, save the helm values for mariadb:
```
helm show values bitnami/mariadb > /tmp/mariadb-values.yaml
```

Change the following and save:
```
auth:
  rootPassword: "password"
  database: wp_db
  username: "user"
  password: "password"
```

Now, install mariadb with the modified helm values:
```
helm install mariadb bitnami/mariadb --values /tmp/mariadb-values.yaml
```

Now, save the helm values for wordpress:
```
helm show values bitnami/wordpress > /tmp/wp-values.yaml
```

Change the following and save:
```
wordpressUsername: user
wordpressPassword: "password"
allowEmptyPassword: false

mariadb:
  enabled: false

externalDatabase:
  host: mariadb
  port: 3306
  user: user
  password: "password"
  database: wp_db
```

Now, install wordpress with modified helm values:
```
helm install wordpress bitnami/wordpress --values /tmp/wp-values.yaml
```

And then apply the wp-ingroute-tls.yaml template from kubernetes yamls directory

---
Q: Can containers reach back to host services via host.docker.internal?
A: Yes. On Windows, you may need to create a firewall rule to allow communication between the host and the container. You can run below command in a privileged powershell to create the firewall rule.

New-NetFirewallRule -DisplayName "WSL" -Direction Inbound -InterfaceAlias "vEthernet (WSL)" -Action Allow

Q: How can I add Rancher Desktop to the startup programs list on Windows?
A: On Windows, you can add a program to startup programs list in different ways. For example, you can use below steps.

- Press Windows+R to open the Run dialog box.
- Type `shell:startup` and then hit Enter to open the Startup folder.
- Copy "Rancher Desktop" shortcut from Desktop and paste in Startup folder.
- Restart your machine.

---
## Kubernetes Cluster on Cloud Providers:

Add minimum 2-3 worker nodes

Download and setup kubectl on host machine

Download the kubeconfig.yml or save the config in a file on host machine
```
export environment variable for kubeconfig:
export KUBECONFIG=~/kubeconfig.yml
```

---
To have all mywebsite reachable through a central address (ip) the deployment needs a service that will load balance and also expose the pods
To achieve this we need another file preferably named mywebsite_service.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  annotations:
    service.beta.kubernetes.io/linode-loadbalancer-throttle: "4"
  labels:
    app: nginx
spec:
  type: LoadBalancer
  ports:
  - name: http
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx
  sessionAffinity: None
```

Important portion on the service yml file are:
```
ports - to expose
selector - to point which pods will be loadbalanced, here the “app” labels will be used
```

#### To apply the service file:
```
kubectl apply -f mywebsite_service.yaml
```

#### To check services:
```
kubectl get services
```

#### VERY IMPORTANT
Once the loadbalancer service is created on the nodes, it creates a NodeBalancer on Cloud provider which will cost money

The nodebalancer external ip is the public ip of the cluster

To see detailed info about the services:
```
kubectl describe services mywebsite_service
```

Finally, to update the running site across all containers running on all pods:

Update the docker image on dockerhub
```
kubectl edit deployment mywebsite 
```
And change the container image name and save
Thats it.. it automatically starts terminating and creating new pods with updated containers.
The loadbalancing updates automatically .

If using NodePort for deployment:
```
kubectl port-forward service/nginx-service 7080:8080
```

### gke cluster creation
 * gcloud auth login --no-launch-browser
 * gcloud components install gke-gcloud-auth-plugin
 * gcloud info --run-diagnostics
 * gcloud container clusters get-credentials kubenetes-engine --zone=uswest2-a  --project=kubernetes-386214  
  
