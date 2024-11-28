let's assume you have a EC2 machine and you deployed your container on it.
how do you know whether it is still alive ? ran out of memory ? 
this is where container orchestration comes into picture.

Container Orchestration:
    1.ECS
        ECS cluster
            managing services

    2.EKS
        EKS Cluster / Control Plane
            master node on every availability zone
            etcd

But still, you need to manage 
    EC2 instances
        OS
        Docker
        ECS agent / K8S processes

Infrastructure managed by AWS
	Fargate
		will take care of EC2 Instance
            OS
            Docker
            ECS agent / K8S processes
		It(EC2) won't be created in your accout, in aws account 


Reference:
    https://www.youtube.com/watch?v=AYAh6YDXuho

Issues with Aws secret manager :
    1. Need to update every workflow in all service repos for getting values from aws 
    2. Cost for secret api call each time we trigger a single workflow
    3. A bit of time will included in workflow to fetch the value
    4. Twer's won't have access to central account to see whether the secret is present or not
    5. Maintenance
        1. Create/Update secret on aws
        2. Delete workflow
        3. IAM permissions for secret management
    6. We should have aws account details somewhere to connect to aws. ( we can't store it it aws since we can't get the details from aws to login to aws )