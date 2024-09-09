pod_scheduler -> CA -> ASG -> Ec2
pod_scheduler -> Karpenter -> Ec2

Advantages

Life Cycle:

    Evaluation
    Implementation
    Day2 Operations

NodePool configuration:

    labels will be added to pod
    node termination handler
    nodepool types:
        single
        multi
        weighted
    Optimization:
        reducing the node of ec2 instances
        reducing the instance type
        consolidation = WhenUnderutilized

Batching:

    1 sec idol window lo enni pods vaste anni flush chestam
    okavela 1 sec ideal time eh lekunda keep on pods vastu unte, max 10 seconds varakuu wait chesi flush cheyyali

Scheduling:

    pod resource requests ( tags can be added which will be automatically added to the nodes by karpenter )
    nodeSelector using lables
    nodeAffinity
    taints and tolerations
    topology spread
    pod_affinity and anti_affinity

Drift:

    when there is a mismatch in provisioned and current config, it will update the nodes in a rolling deployment fashion
    latest ami version
    it will select the AMI from ASM parameter store
    ami version based on eks version

Distruption:

    Select candidate -> Launch Replace -> Drain candidate -> Delete condidate
    have control over
        you can add config to not distrupt a perticuler pod
        how many pods can distrupt at a time
        how speed the distruption can happen
    
    while taking the decision to find out the candidate it will try to focus on minimizing the pod distruption.
    it will consider whether the pod can be distrubed or not

Consolidation:

    N -> 0 consolidation ( no new nodes )
    N -> 1 consolidaiton ( new nodes will be created )

Can't run karpenter on node which is provisioned by karpenter.
prometheus + grafana setup since it emits the metrics

scheduling constraints:

    node affinities
    taints & tolerations
    topology spread
    pod affinity
    persistent volume topology
    pod distruption budget
    distruption control
        do-not-distrupt: true


Document:

    what is karpenter ?
        Karpenter is an open-source node lifecycle management project built for Kubernetes. We are planning to use Karpenter to manage AWS EKS managed nodes in AWS EKS cluster. Karpenter will take care of autoscaling nodes depending upon the predefined metrics. You can get more information about Karpenter by following below link: Karpenter Docs
    
    Why we need this in our architecture ?
        Earlier days we are using eks fargate for our workloads where it will take care of scaling the worker nodes based on-demand. But Currently we will be moving away from fargate moving towards the EC2 nodes ( node groups ) for our workloads. So we need a tool to scale our worker nodes based on the load. This connects me to two approaches
        1. Cluster auto scaler
            The Kubernetes Cluster Autoscaler automatically adjusts the number of nodes in your cluster based on the load by using the Auto Scaling groups. Though it's providing the functionality to scale our worker nodes it also comes with some limitations like you can't scale out beyond the asg configuration and sometimes it will end up with underutilized nodes.
        2. Karpenter
            It's far matured then legacy cluster auto scaler. It does more then auto scaling. One of the feature of karpenter is, it will consolidate the nodes when there was an under-utilized nodes. It will drastically decrease our cost.
    
    How to adopt Karpenter ?
        To set up karpenter in our architecture we need to configure 3 components. Where should I install these things ? well, aws is recommending to install these on either on fargate or 2-node node group. You can see out 2-node node group HERE.
        1. Karpenter controller :
            It's a main component of karpenter which will take care of Provisioning Nodes, Optimizing Node Utilization, Monitoring and Adjusting.
            Install:
                Well, we have a helm chart to install the controller quickly. 
    
                helm upgrade --install karpenter oci://public.ecr.aws/karpenter/karpenter --version "${KARPENTER_VERSION}" --namespace "${KARPENTER_NAMESPACE}" --create-namespace \
                --set "serviceAccount.annotations.eks\.amazonaws\.com/role-arn=${KARPENTER_IAM_ROLE_ARN}" \
                --set "settings.clusterName=${CLUSTER_NAME}"
    
                wait, here there are two things to install from ourside.
                1. namespace to install our components. you can find out namespace HERE.
                2. service-account role to give least previledge permissions to the controller. you can find out policy & role HERE.
        
                check the status of your karpenter controller by using this command ` kubectl get pods -n karpenter-controller`
            
        2. NodePool
            YAML configuration which is having the info about What kind of Ec2 instances the karpenter to provision.ex, which type of instance and what size of instance to provision. You can find our nodepool configuration HERE.
            In our case we will be having two types of node-pools. One is for gha-runners and another one is for all except gha-runners. I told you earlier nodepool is something like what kind of ec2 instances, so for gha-runners we need a differnt type of ec2 instance where we need compute optimized and we don't want any interruptions / consolidation because gha pods should not have any interruptions until the job gets completed.
    
        3. NodeClass
            YAML configuration which talks about more towards the ec2 machine. It's more likely equal to laungh template. ex, what kind of AMI to use, which security groups and subnets to attach to EC2. You can find our claanode configuration HERE.
    
        
    How to migrate workloads from fargate to karpenter nodepool ?
        Right now we have our general workloads configured in fargate here and gha workloads on ec2 node group here.
        
        First Approach :
            We decommissioned the fargate profile by removing the code in an assumtion that the pods will move to the either ec2 node group / karpenter nodepool in rolling deployment fashion. Then we can use nodeSelector to the service deployment to force the deployment to go under karpenter. But it's not working as what we expected. Removing fargate profile leads to removing the fargate nodes evetually removing the pods under the fargate node. within 5 seconds of downtime the new pods got created in ec2 workload ( node group / nodepool ).
        
        Second Approach :
            The motive behind this approach is, from the first approach we got to that we should not touch the fargate profile because it's giving us downtime. We should find a way to move our workloads from fargate to ec2 without decommissioning the fargate profiles. This leads to the below approach.
            
            1. Inform in General thread to not trigger the deployment workflow / any rollout restart during a specific time period. ( If possible Disable Deployment workflow for prod )

            2. patch all the deployments under the namespace to migrate from fargate to node group without any downtime. ( command: k patch deployment deployment_name -n namespace_name -p '{"spec": {"template":{"metadata":{"annotations":{"eks.amazonaws.com/compute-type":"ec2"}}}}}' )
            
            3. do some sanity checks from our end & development teams ( for prod, do this on call with them )
            
            4. remove the fargate profile from the fargate_profile variables, and add it to the additional_namespaces so that only fargate profile will be decommissioned not the namespace. check out this PR for reference. 
            
            5. add nodeSelector ( node_pool:karpenter ) to deployment so that pods will be moved from node group to the node pool without any downtime 
            
            6. do some sanity checks from our end & development teams ( for prod, do this on call with them )
            
            7. Inform in General thread that their deployment workflows are free to trigger. ( Enable the Deployment workflow )

    Issues we got while rollout the karpenter 
        During the Karpenter rollout, we experienced 2-3 seconds of traffic fluctuations despite having at least one pod running to handle the load. Initially, we suspected this was an issue with Karpenter. However, when we conducted the same activity with non-Karpenter deployments, we observed the following: