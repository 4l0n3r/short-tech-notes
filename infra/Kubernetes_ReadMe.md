							Kuberenetes

Matrix from Hell :
	if you wanna change one component version then you need to resolve dependency issues with other components + underlying os.

Docker :
	app
	lib,dep
	docker
	os
	hardware

	less in size
	less boot time
	low isolation since its sharing the same kernal for all containers. but not in hypervisor because of using different os kernals.

Hypervisor :
	app
	lib,dep
	os
	hypervisor
	os
	hardware

Dev												Operational team
installation guide + app code					confused ( since they didn't developed the code )
App code + Dockerfile = Docker Image 			install with out any issues

container archestration : Kubernetes
	deploying and managing hunrdered and thousands of containers in a clustered environment.

Kubernetes:
	API : will act as a frontend to the kubernetes
	ETCD : will store the cluster nodes data in a key-value pair manner. it is responsible for implementing locks inside the cluster to avoid conflicts
	Scheduler : will take care of distributing work across containers.
	Controllers : brain in k8s. noticing and responding when nodes goes down. and make decision to create new nodes.
	Container Runtime : Docker,  Underlying software that is used to run containers. 
	Kubelet : agent installed on each node in clusted. making sure that the containers are running in nodes as expected or nor.

Pod :
	smallest object that we can create in kubernetes
	one to one relationship with container.We can't create the same container inside the Pod.
Multi - Pods :
	We can create helper containers inside the same pod.

Why Pods :
	let's assume we need run a python app with some helper api
	python-1	python-2	python-3	python-4
	helper-1	helper-2	helper-3	helper-4
	we need to manage many things here :
		create network between python, helper
		create a common storage
		store the mapping details
		if python dies we should go and delete the helper too
		
	Pod will do this things for us and make our lives easy.

Kubectl create -f pod-definition.yml
Kubectl apply -f pod.yml ( after changing something on file )
Kubectl run redis —image=redis123 —dry-run=client -o yaml > pod.yaml
Kubectl edit pod redis
Kubectl delete -f pod.yaml

Kubectl get pods
Kubectl get nods
Kubectl get nodes -o wide

Kubectl describe pods
Kubectl describe pod podName

Replication Controller == Replication Set :
	will take care of creating pod if something got crashed.

kubectl create -f replicaset-def.yml
kubectl get replicaset
kubectl delete replicaset replicaset-name
kubectl replace -f replicaset-definition.yml ( after changing something on file )
kubectl scale --replicas=6 replicaset replicaset-name
kubectl scale --replicas=6 -f replicaset-def.yml

Deployments:
	capability to upgrade the underlying instances seamlessly using rolling updates.
	Deployment will create
		ReplicaSet will create
			Pods
				will use image to create container.
	Recreate :	destroy,update all at a time.
	Rolling Update: destroy and update one by one

	whenever we update the deployment-definition.yml file this is what will happen Rolling Update.

	kubectl create -f deployments-definition.yml
	kubectl get deployments
	kubectl apply -f deployment-definition.yml
	kubectl set image deployment/deployment-name nginx=nginx:1.9.1
	kubectl rollout status deployment/deployment-name
	kubectl rollout history deployment/deployment-name
	kubectl rollout undo deployment/deployment-name

Service:
	why service	:
		pod is ephemeral means, destroys frequently. it will generate new ip in each time it dies.
		service will provide the static ip address. even the pod dies the ip is same for service to access.
		
	expose the application to outside world.
	even if you create more than one pod with same labels ( even on different nodes ) service lables it will take care of selecting those pods and split the network based on load.

	NodePort:

apiVersion :
Kind :
Metadata:
	name:
	labels:
		app:
		type
Spec:
	containers:
		- name : 
		   image: 
		   env :
			name: 
			value:
		- name :
		   image: 
	

Kubectl version

CRUD COMMANDS :
Kubectl create deployment deployment_name —image=imageName 
kubectl run nginx --image=nginx
Kubectl edit deployment deployment_name
Kubectl delete deployment deployment_name

STATUS COMMANDS:
Kubectl get deployments
Kubectl get deployment deployment_name
Kubectl describe deployment deployment_name

DEBUGGING COMMANDS:
Kubectl logs deploymentdeployment_name 
Kubectl exec -it PodName — bin/bash

Kubectl describe replicaset deployment_name
Kubectl edit replicaset deployment_name
Kubectl scale replicaset deployment_name —replicas=3

Kubectl get replicasets.apps
Kubectl get replicaset deployment_name
Kubectl describe replicaset deployment_name
Kubectl delete replicaset deployment_name

Kubectl get all  => return pods , services , replicas , deployments

without control manager -> deployment wont create replicaset.
without scheduler -> resplicaset wont create pods since it doesnt know where to schedule.

version error -> kubelet not connected to api server
node not ready -> kubelet stopped

CNI -> manage all networking things ( CIDR )


Services to be installed inside the eks cluster :

	Core-dns :
		DNS Server: 
			CoreDNS acts as a DNS server, providing name resolution for domain names within the Kubernetes cluster. It responds to DNS queries, translating domain names to IP addresses, and vice versa, just like a traditional DNS server.
		Service Discovery: 
			As part of its integration with Kubernetes, CoreDNS provides service discovery capabilities. It allows applications and services within the cluster to discover other services by their DNS names, making it easier for microservices to communicate with each other.
		Caching and Forwarding: 
			CoreDNS supports caching of DNS records to improve performance and reduce the load on upstream DNS servers. It can also forward DNS queries to external DNS servers for resolution of external domains.
		Load Balancing: 
			CoreDNS supports load balancing of DNS queries among multiple backend servers, helping distribute the load and provide high availability.
	Reloader:
		Reloader is an open-source tool designed to enhance the automation of Kubernetes application updates by triggering the rolling restart of Pods when ConfigMap or Secret resources are updated. It aims to simplify the process of propagating configuration changes to running applications without manual intervention.

		Reloader monitors changes to ConfigMap and Secret resources within a Kubernetes cluster. When changes occur, it automatically triggers rolling restarts of Pods that use these resources. This ensures that applications consume the updated configuration without manual intervention.
	

affinities:

	node affinity: 
		we can direct the scheduler to only place certain pods on nodes with a specific label or nodes in a certain availability zone
	pod affinity:
		Pod affinity allows us to set priorities for which nodes to place our pods based off the attributes of other pods running on those nodes
	pod anti-affinity:
		ensuring certain pods don’t run on the same node as other pods

doubts :
	dependency, mock ouotput

suggestions :
	we should add prevent_destroy lifecycle to namespace to avoid destroying it



