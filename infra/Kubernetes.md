    						Kuberenetes

container archestration :

    Kubernetes - > deploying and managing hunrdered and thousands of containers in a clustered environment.

Kubernetes:

    API : will act as a frontend to the kubernetes
    ETCD : will store the cluster nodes data in a key-value pair manner. it is responsible for implementing locks inside the cluster to avoid conflicts
    Scheduler : will take care of distributing work across containers.
    Controllers : brain in k8s. noticing and responding when nodes goes down. and make decision to create new nodes.
    Container Runtime : Docker, Underlying software that is used to run containers.
    Kubelet : agent installed on each node in clusted. making sure that the containers are running in nodes as expected or nor.

Pod :

    smallest object that we can create in kubernetes
    one to one relationship with container.We can't create the same container inside the Pod.

Multi - Pods : We can create helper containers inside the same pod.

Why Pods :

    let's assume we need run a python app with some helper api
    python-1 python-2 python-3 python-4
    helper-1 helper-2 helper-3 helper-4
    we need to manage many things here :
    create network between python, helper
    create a common storage
    store the mapping details
    if python dies we should go and delete the helper too
    Pod will do this things for us and make our lives easy.

Replication Controller == Replication Set : will take care of creating pod if something got crashed.

Deployments:

    capability to upgrade the underlying instances seamlessly using rolling updates.
    Deployment will create
    	ReplicaSet will create
    		Pods
    			will use image to create container.
    Recreate : destroy,update all at a time.
    Rolling Update: destroy and update one by one

    whenever we update the deployment-definition.yml file this is what will happen Rolling Update.

Service:

    why service :
    	pod is ephemeral means, destroys frequently. it will generate new ip in each time it dies.
    	service will provide the static ip address. even the pod dies the ip is same for service to access.
    	expose the application to outside world.
    	even if you create more than one pod with same labels ( even on different nodes ) service lables it will take care of selecting those pods and split the network based on load.

    to access your application which deployed as a pod and exposed using a service, you just need to specify the service name. It is resolved to the service's IP address by CoreDNS, which is deployed with an Amazon EKS cluster, by default. For that you should be inside the cluster first ( maybe inside any pod )

    clusterIp:
    	default
    	only for internal communication

    NodePort:
    	exposes a service on a static port on each node
    	not providing same level of load balancing and service discovery and network security as a LoadBalancer service
    	service will expose an IP that will again forward the traffic to each node_ip:static_port

    LoadBalancer:
    	provision alb on cloud
    	load balancer + service discovery + service security
    	user -> alb -> service -> deployment

    Ingress ( Out of topic )
    	needs ingress controller to set it up
    	will create lb on cloud
    	used to route traffic to multiple services based on path & hostname

Volumes:

    Pod is ephemeral, so whatever you store inside the pod will be lost once the pod got restarted. This is where we use Volumes for some extent.

    emptyDir:
        this is just to have common storage between two containers inside the pod itself. will lost with pod restart
    hostPath:
        the storage will be created at node level. lost data with node distruption and pod schedules on different node.
    awsElasticBlockStore:
        need to add volumeId to the manifest file. who creates and adds id to the manifest file.

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

    	nameserver 10.100.0.10 default namesercer assigned to all pods

    Reloader:
    	Reloader is an open-source tool designed to enhance the automation of Kubernetes application updates by triggering the rolling restart of Pods when ConfigMap or Secret resources are updated. It aims to simplify the process of propagating configuration changes to running applications without manual intervention.

    	Reloader monitors changes to ConfigMap and Secret resources within a Kubernetes cluster. When changes occur, it automatically triggers rolling restarts of Pods that use these resources. This ensures that applications consume the updated configuration without manual intervention.

scheduling constraints:

    affinities:

    	node affinity:
    		we can direct the scheduler to only place certain pods on nodes with a specific label or nodes in a certain availability zone
    	pod affinity:
    		Pod affinity allows us to set priorities for which nodes to place our pods based on the attributes of other pods running on those nodes
    	pod anti-affinity:
    		ensuring certain pods don’t run on the same node as other pods

    topology spread:

    	Topology spread in Kubernetes refers to the distribution of pods across different topological domains (like nodes, zones, or regions) in a cluster. The goal of topology spread is to ensure that workloads are evenly distributed across these domains, which enhances fault tolerance, high availability, and resource utilization.

    	persistent volume topology

    taints & tolerations:

    	nodes will have taints to allow only few pods to be schedueled
    	pods will tolerations to have permissions to deploy on tainted nodes

    pod distruction budget:
    	A Pod Disruption Budget (PDB) in Kubernetes is a policy that defines the minimum number or percentage of Pods in a deployment that must remain available during voluntary disruptions.
    	Pods will never be forcibly deleted, so pods that fail to shut down will prevent a node from deprovisioning.
    	will block node termination if evicting the pod would cross the pdb.

service account:

    k8s component which is attached to aws world policies and roles.
    restricted to namespace
    It works via IAM OpenID Connect Provider (OIDC) that EKS exposes, and IAM Roles must be constructed with reference to the IAM OIDC Provider
    Inside EKS, there is an admission controller that injects AWS session credentials into pods respectively of the roles based on the annotation on the Service Account used by the pod

Statefulsets:

    A StatefulSet is a Kubernetes resource used to manage stateful applications. Unlike Deployments, which are used for stateless applications, StatefulSets are designed for applications that require persistent storage, stable network identities, and ordered, predictable deployment and scaling.
    Key Features of StatefulSets:
    	1. Stable Network Identity: Each Pod in a StatefulSet gets a stable, unique hostname. For example, if you have 3 Pods in the StatefulSet, they will have names like: (my-app-0,my-app-1,my-app-2)
    	2. Stable Storage: StatefulSets use PersistentVolumeClaims (PVCs) to ensure that each Pod gets its own persistent storage. When a Pod is rescheduled, it gets the same volume it had before.
    	3. Ordered Deployment and Scaling: Pods in a StatefulSet are created and terminated in a specific order (e.g., my-app-0 is created first, then my-app-1, and so on). This is important for applications like databases, where one instance must be started before another.
    	4. Graceful Termination: When scaling down or deleting Pods, StatefulSets ensure that Pods are terminated in reverse order (e.g., my-app-2 is terminated first, then my-app-1).

eks:

    eksctl creates default resources along with cluster:

    	cluster
    	vpc
    		public subnets 2
    		private subnets 2
    		route tables
    		security groups
    		InternetGateway
    		NatGateway
    	addons
    		vpc-cni
    		kube-proxy
    		coredns
    	addon roles
    	no oidc -> so no attachment for addon roles
    	no cloudwatch logs
    	kubeconfig file

    fields to create cluster:

    	metadata
    	vpc
    	managedNodeGroups
    	nodegroup
    	iam
    	addons
    	accessConfig
    		authenticationMode
    	outpost

    eks Pod Identity Associations:

    	no need of OIDC ( specific to cluster )
    	so we can have same role for all namespaces
    	it can also added to eks add-ons ( either auto apply role / specific policy apply )
    	we can migrate existing iamserviceaccounts and addons to pod identity associations using a single command

    we can register non-eks cluster with eks connector
    we can manage kubelet configuration -> I mean how kubelet should work
    cloudwatch logging
    fully private cluster
    addon
    fargate
    cluster upgrade:
    	controle plane
    	addons
    	nodes

Webhooks:

    MutatingAdmissionWebhook:
        Used to modify or mutate a resource before it is persisted in etcd. Example: Adding a default sidecar container to pods when deployed (like App Mesh injectors).
    ValidatingAdmissionWebhook:
        Used to validate the resource's configuration. Example: Ensuring that all resources confirm to specific naming conventions or mandatory fields.

Commands:

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
    kubectl create -f replicaset-def.yml
    kubectl get replicaset
    kubectl delete replicaset replicaset-name
    kubectl replace -f replicaset-definition.yml ( after changing something on file )
    kubectl scale --replicas=6 replicaset replicaset-name
    kubectl scale --replicas=6 -f replicaset-def.yml

    kubectl create -f deployments-definition.yml
    kubectl get deployments
    kubectl apply -f deployment-definition.yml
    kubectl set image deployment/deployment-name nginx=nginx:1.9.1
    kubectl rollout status deployment/deployment-name
    kubectl rollout history deployment/deployment-name
    kubectl rollout undo deployment/deployment-name


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
