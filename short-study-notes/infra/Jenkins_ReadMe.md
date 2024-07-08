																						JENKINS



WaterFall Model 
Agile  -> addressed gap between client and developer
DevOps  -> addressed gap between developer and operation team

Architecture - Distributed :
	multiple files ( html , py , js ) need multiple builds , but Jenkins doesn’t allows multiple builds so we need distributed Jenkins architecture to over come this problem.

What :
	developer -> continues commit 
	Jenkins	 -> continuous pull  , build 
	CI ( merging all devs code ) 
	CD ( deliver at any time )
	tool
	Build and Deliver software products

To Do :
	pull code
	build
	validate
	review
	test 
		unit test
		integration test
	packaging application
	deploy 

Node JS :
	event-driven 
	asynchronous I/O
	very fast response


Normal Job 
Periodic Job
SimpleGit Job
Maven Job
											

Jenkinsfile :

Pipeline {
	agent any
	tools{
		
	}
	parameters {
		string(name, defaultValue, description)
		choice(name, choices, description)
		booleanParam(name, defaultValue, description)
	}
	stages {
		when {
			expression {
				condition
			}
		}
		stage “build” {
			steps {
			
			}
		}
	}
	post {
		always {
		
		}
		success{

		}
		failure{

		}
	}
}