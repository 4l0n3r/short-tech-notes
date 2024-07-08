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