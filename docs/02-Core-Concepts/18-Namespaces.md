# Namespaces
  - Take me to [Video Tutorial](https://kodekloud.com/topic/namespaces/)
  
In this section, we will take a look at **`Namespaces`**

So far in this course we have created **`Objects`** such as **`PODs`**, **`Deployments`** and **`Services`** in our cluster. Whatever we have been doing we have been doing in a **`NAMESPACE`**.
- This namespace is the **`default`** namespace in kubernetes. It is automatically created when kubernetes is setup initially.

There are two boys named Mark. To differentiate them from each other,we call them by their last names, Smith and Williams. They come from different houses.Of course, the Smiths and the Williams.There are other members in the house.The individuals within the house address each other simply by their first names. For example, the father addresses Mark, simply as Mark. However, if the father wishes to address the Mark in the other house, he would use the full name. Someone outside of these houses would also use the full name to refer to the boys or anyone within these houses. Each of these houses have their own set of rules that defines who does what. Each of these houses have their own set of resources that they can consume.

 Kubernetes creates a set of pods and services for its internal purpose, such as those required by the networking solution, the DNS service, etcetera. To isolate these from the user and to prevent you from accidentally deleting or modifying these services, 
 Kubernetes creates them under another name space created at cluster startup named kube-system.
 A third name space created by Kubernetes automatically is called kube-public. This is where resources that should be made available to all users are created. 
 
 We can create our own name space and have set of policies for them, they can have their separate quota.
 
 With in the cluster we can directly access the service but outside the cluster we need to follow below type of format 
 use the servicename.namespace.svc.cluster.local format. 
 That would be dbservice.dev.svc.cluster.local. You're able to do this because when the service is created, a DNS entry is added automatically in this format. 

 <img width="448" alt="image" src="https://github.com/Pradyumnyadav0992/certified-kubernetes-administrator-course/assets/94163028/8d6dfa06-b33f-47f0-a1aa-77972fc4cd4d">

 
 Looking closely at the DNS name of the service, 
 the last part, cluster.local, is the default domain name of the Kubernetes cluster. 
 SVC is the sub domain for service followed by the name space, and then the name of the service itself. 
 



  ![ns](../../images/ns.PNG)
 
- You can create your own namespaces as well.

  ![ns3](../../images/ns3.PNG)
  
- To list the pods in default namespace
  ```
  $ kubectl get pods
  ```
- To list the pods in another namespace. Use **`kubectl get pods`** command along with the **`--namespace`** flag or argument.
  ```
  $ kubectl get pods --namespace=kube-system
  ```
  ![ns8](../../images/ns8.PNG)
  
- Here we have a pod definition file, when we create a pod with pod-definition file, the pod is created in the default namespace.

```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
     app: myapp
     type: front-end
spec:
  containers:
  - name: nginx-container
    image: nginx
 ```
  ```
  $ kubectl create -f pod-definition.yaml
  ```
- To create the pod with the pod-definition file in another namespace, use the **`--namespace`** option.
  ```
  $ kubectl create -f pod-definition.yaml --namespace=dev
  ```
  ![ns9](../../images/ns9.PNG)

- If you want to make sure that this pod gets you created in the **`dev`** env all the time, even if you don't specify in the command line, you can move the **`--namespace`** definition into the pod-definition file.
```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  namespace: dev
  labels:
     app: myapp
     type: front-end
spec:
  containers:
  - name: nginx-container
    image: nginx
 ```
  
  ![ns10](../../images/ns10.PNG)
  
- To create a new namespace, create a namespace definition as shown below and then run **`kubectl create`**
```
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

  ```
  $ kubectl create -f namespace-dev.yaml
  ```
  Another way to create a namespace
  ```
  $ kubectl create namespace dev
  ```
  ![ns11](../../images/ns11.PNG)
  
- By default, we will be in a **`default`** namespace. To switch to a particular namespace permenently run the below command.
  ```
  $ kubectl config set-context $(kubectl config current-context) --namespace=dev
  ```
- To view pods in all namespaces
  ```
  $ kubectl get pods --all-namespaces
  ```
  ![ns12](../../images/ns12.PNG)
  
- To limit resources in a namespace, create a resource quota. To create one start with **`ResourceQuota`** definition file.
```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memory: 10Gi
```
  ```
  $ kubectl create -f compute-quota.yaml
  ```
  ![ns13](../../images/ns13.PNG)
  
K8s Reference Docs:
- https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
- https://kubernetes.io/docs/tasks/administer-cluster/namespaces-walkthrough/
- https://kubernetes.io/docs/tasks/administer-cluster/namespaces/
- https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/quota-memory-cpu-namespace/
- https://kubernetes.io/docs/tasks/access-application-cluster/list-all-running-container-images/
  
  
