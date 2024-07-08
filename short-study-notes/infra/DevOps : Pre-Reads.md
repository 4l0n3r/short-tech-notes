DevOps : Pre-Reads

Cloud Computing
	https://www.youtube.com/watch?v=M988_fsOSWo&t=295s
	https://www.youtube.com/watch?v=mxT233EdY5c&t=125s
	https://www.youtube.com/watch?v=1BMO7YkwR6Y&t=2s

Introduction to AWS
	https://www.youtube.com/watch?v=Z3SYDTMP3ME

Cloud-Native Apps
	https://www.youtube.com/watch?v=osz-MT3AxqA

CI / CD
	https://www.youtube.com/watch?v=HjXTSbXG1k8

Feature toggles and Trunk Based
	https://www.youtube.com/watch?v=0jM2c16DjuA
	
	Branching Strategy :
		feature 1 branch
			sub feature branch
		feature 2 branch
		
		Having high number of branches is not a problem. The main problem is when merging the branches. Solution is Trunk based Development.

	Trunk Based Development :
		Small code changes go to main branch regularly
	
	Feature Toggles / Features tags
	
	Implementing features toggles :
		Using If conditions
		
		If feature 1:
			Do this
		It is good for simple toggles. But what if we have sub features. It will make a triangle.It is called TOGGLE HELL OR TRIANGLE OF DOOM. 
	
DockerFile is a blue print for building docker Image , 
DockerImage is a template for running docker Container ,
DockerContainer is simply a running process.

Docker Volume :
	to share containers to others.
Docker Compose :
	 to run multiple docker containers at same time 

Container (Virtual Machine) : Programs running inside of a container can only see the container contents and devices assigned to it.
	Why containers :
		1.Development and Production has diff environments
		2.Employee 1 and Emp 2 has diff environments in a team
		3.Diff projects needs diff environments
	Container Vs Virtual Machine
	Low impact on Os
	Easy Sharing 
	Encapsulate only environment instead of whole machine

Os-Level Virtualization :  Kernel allows the existence of multiple containers

Linux Container : Os-level virtualisation method for running multiple isolated linux system in one host linux kernel.