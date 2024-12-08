																			Docker

Matrix from Hell :

	if you wanna change one component version then you need to resolve dependency issues with other components + underlying os.

Docker :

    app
    lib,dep
    docker
    os
    hardware
	
	pros:
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

Dev Operational team:

    installation guide + app code confused ( since they didn't developed the code )
    App code + Dockerfile = Docker Image install with out any issues

docker run
	name
	itd
	port
	volume
	env
	
	logs
	inspect

docker-compose:
	we can also mention the Dockerfile path instead of docker image while creating the docker-compose file.
	docker-compose: version-1
		1. in default network, so you need to mention the link btw the containers because it might be possible that there are already some instance in default network.
		2. no order

	docker-compose: version-2
		1. it will create a dedicated network first then create the services inside it. so no need to mention the link property here. it will automatilly connect to that.
		2. depends_on

cgroupds:
	there is no limit for container to use cpu,memory of hosted system.
	we can restrict it using cgroups.

docker-storage:
	var/lib/docker ( the structure will based on the storage driver that we are using )
		images
		containers
		volumes
	
	docker history imagename
	docker system df
	docker system df -v

docker network: ( Same as volume commands )

	bridge
		each container will have a diff ip, we can use same port.
	null
		container not attached to any network.
	network
		in the same machine all containers will be created. the port should be different.
	
	docker run --network=none imagename
	docker network 
		create 
			--driver bridge 
			--subnet 182.18.0.1/24 
			--gateway 182.18.0.1 
		wp-mysql-network
	
	docker run --name webapp -e DB_Host=mysql-db -e DB_Password=db_pass123 -p 38080:8080 --network=wp-mysql-network --link mysql-db kodekloud/simple-webapp-mysql
	
	docker network disconnect my-net my-nginx


Docker services:
	when :
		when there need of accessing one service in another service

	Node Port  :  external service  ( exposed to outside world )
	ClusterIp  : internal service ( new ip will be created for the service to access )
	Load Balancer  : 

docker engine:
	the process running inside the container is nothing but the process running on docker host. only the process id will be different.

what is distroless images ?

	A distroless Docker image is a minimalistic container image that includes only the necessary runtime dependencies required to run an application, without OS package manager.Distroless images are designed to be small, secure, and focused on a specific application.
	1. No Package Manager or Shell
	2. Smaller Image Size
	3. Improved Security
	4. Focused Runtime Environment
	5. Read-Only File System
	When security & lightweight image matters use distroless, when you need to debug frequently and application relies on dynamic installations don't use it.

Basic commands: 

 	docker build -t image_name .
 	docker run -itd --name container_name image_nme
	docker exec -itd container_name bash
	docker cp source_folder container_name:destination_folder

	docker ps
		to see all the  containers running
	
	docker ps -a
		to see all the  containers including stoped containers
	
	docker stop cname
		To stop a container
	
	docker rm cname
		To remove the container
	
	docker run container 
		if not present in local repo then it will search in docker hub and it will download it.
		if you again run same command it won't download anything.
	
	docker run -it node 
		i -> ( for taking inputs )
		t -> ( for printing the prompt )

	Attached mode :
		docker run -p 8000:30 id 
		docker start -a name
		docker attach name 	

	Detached mode :
		docker start name	 
		docker run -p 3000:80 -d id
	
	Interactive mode  : for taking input
		docker run -it name
		docker start -a -i name
	
	Logs :
		docker logs cname         =>  prints all
		docker logs -f cname     =>  print and keep on listening
	
	Remove :
		docker rm Con_name1 Con_name2 Con_name3
		docker rmi imagename1 imagename2
		docker image prune  => delete unused images
	
	Automatically Remove Container : After execution
		docker run -p 8000:80 -d —rm id
	
	Details of Image :
		docker image inspect image
	
	Copy :
		docker cp local_source cname:path_in_container
		docker cp cname:file_in_container local_destination
	
	Tags & Names :
		docker build -t tag : name .
		docker tag old_name:tag  new_name:tag
		Container  :
		docker run -—name containerName

Diff between run and cmd
	cmd won't be executed when the image was created , it will execute only when the container is started based on image.
	run will be executed while building the image.
	cmd will be executed while running the container.

EXPOSE is still optional because we need to expose port using -p while running the docker

No need to write the full id , we can write the first few characters to identify it.

Docker is READ-ONLY
Container is READ-WRITE

Docker Layers :
	Each instruction is a Layer
	When you re-build the image, only the instructions where something changed and all instructions after are re-evaluated.
	If one layer is updated then the subsequent layers will run from scratch without using Cache.

Docker start cname	=> will restart the already stoped container  without blocking the terminal.



Sharing Images and Containers
	Don’t share containers , share only Image.
	If you have image file then you can create containers based on it.
	Share Dockerfile or Share a build image

DockerHub :
	name of an image and repository in dockerHub should be same.
	change name of image using tag command.

Push & Pull :
	docker push username/reponame
	docker pull username/reponame

Volumes :
	separate storage area for container data.
	decoupling container from docker.
	deleting container will not effect volume.
	we can share it among different containers.
	
	docker volume create volume1
	docker volume inspect volume1
	docker volume prune
	docker volume rm volume1
	docker run —name ContainerName-1  -v  volume1:/var/jenkins_home -p 8080:8080 -p 5000:5000
	docker run —name ContainerName-2  -v  volume1:/var/jenkins_home -p 9090:8080 -p 4000:5000

Docker Swarm :
	group of machines which are running docker and joined into cluster.
	one manager node(docker-machine) operating the other slave nodes(docker-machine).

	Install docker-machine
	install virtualbox
	
	docker-machine create —driver virtualbox manager1
	
	docker-machine env manager1
	docker-machine ls
	docker-machine ip manager1

	docker-machine ssh ip

	docker swarm init —advertise-addr MANAGER_IP
	docker node ls

	docker swarm join-token worker		->    It will give you the command , so that you can run it on worker node to join as a worker to manager node

	docker info
	docker swarm 

Docker Compose :
	create docker-compose.yaml 

	docker-compose config

	docker-compose up -d

	docker-compose down 

	docker-compose up -d —scale database=3


			Doubts :
Firewall ? 