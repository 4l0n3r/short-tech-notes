																							TERRAFORM
provider block:
	to declare cloud provider information
	we can create many provider blocks with alias concept -> for deploying in different regions
	can use input values but cann't reference reosurces
	
terraform block:
	block type is used to configure some behaviors of Terraform itself
	cann't use input values & cann't reference reosurces

Provision infrastructure :
	using aws console
	coding in python, bash
	terraform
		multi providers
	ansible using yaml 

terraform state : current state , code : desired state
resource

<block>  <resource_type> <resource_name>
		 <provider = local>
		 <resource = file>

resource "local_file" "pet" {
	filename = "path"
	content = "I love pets"
}

Terraform follows an immutable infrastructure approach
-/+ destroying and creating new one


terraform init
	download plugins for provider
	.terraform/plugins
	registry.terraform.io / hashicorp / local
	hostname			 / namespaece / provider

terraform validate
terraform fmt
terraform plan -out file
terraform apply file
terraform show
terraform show -json
terraform output
terraform output var_name
terraform refresh
	won't change any resource, just updates the state file.


local_sensitive_file

variable.txt
	
	variable types :
		string 
		integer
		bool 
		
		list
		tuple -> diff types
		set
		
		map
		object -> diff types
	
	type constraints to check the type on plan command

command line arguments
environment
terraform.tfvars
*auto.tfvars
-var or -var-file

resource attributes
	having one resource attribute value in another resource -> implicit dependency
depends_on -> explicit dependency

terraform state:
	metadata : to get the details of dependencies while deleting the dependent resources
	performance : no need to fetch the state of every resource from every provider, it can refer to the state file ( truth )
	collaboration : 

	Note:
		it will store every bit of info about resource, even initial password for database. So store it securely.
		don't give permissions to edit this state file. If you wanna update it use terraform commands to achieve it.

lifecycle :
	how can we prevent the resource to create first then destroy next.

	create_before_destroy
	prevent_destroy
	ignore_changes

data_source
	<block> : data
	only to read

count
	filename=var.filename[count.index]
	count=length(var.filenames)

	store elememts in a list
for_each
	only applicable for set, map
	filename=each.value
	for_each=toset(var.filenames)

	store elements as map

lookup
	lookup(map , key)

provider.tf
	region
	access_key
	secret_key

	or add this directly to .aws/credential file
	or use EXPORT

policy :
	<<EOF
		data ...!
	EOF

	file(json_filename)


remote state :
	terraform {
		backend "s3" {
			bucket
			key
			region
			dynamodb
		}
	}

terraform state list
terraform state show resource_address
terraform state mv old_address new_address  -> will update the resource config
terraform state pull
terraform state rm resource_address

local-exec :
	on_failure : fail / continue
	when : destroy

debugging :
	export TF_LOG = TRACE
	export TF_LOG_PATH = PATH

impprt :
	terraform import resource_type.resource_name id
	you should add the config for this resource to configuration file otherwise it will fail.










----------------------------------------------------------------------------------------------------------------------------------------------------------------

Check Time  & Update It

Software Provisioning 

	- creating own AMI and bundle software with it 
	   by using Packer
	-  use standard AMI and install software on it
	    by using Unusable

	File uploads :
		provisioner “file” {
			source 		
			destination 
		}

	SSH values :
		connection {
        			user = "${var.INSTANCE_USERNAME}"
        			private_key = "${var.INSTANCE_PASSWORD}”
   		 }

	SSH Values using key_pairs :
		resource "aws_key_pair" "my_aws_key" {
   			key_name = "mykey"
   	 		public_key = "${file("${var.PATH_TO_PUBLIC_KEY}")}"
		}

		connection {
        			user = "${var.INSTANCE_USERNAME}"
        			private_key = "${file("${var.PATH_TO_PRIVATE_KEY}")}"
   		 }

	Remote Execution :
		resource “aws_instance” “example” {
			ami
			instance_type
			key_name
			provisioner “file” {
				source
				destination
			}
			provisioner “exec” {
				inline = [“command-1” , “command-2”]
			}
			provisioner “local-exec” {
				command = “command1” 			}
		}

	Output :
		output “outputName” {
			value = “${aws_instance.example.public_ip}”
		}

	Remote State:
		current state to desired state
		Backup :
			
			terraform {
				backend “s3” {
					bucket 
					key
					region
				}
			}
			
			data "terraform_remote_state" "aws-state" {
    				backend = "s3"
    				config = {
     					bucket = "terraform-state"
      					key = "terraform.tfstate"
      					access_key = "value"
    					secret_key = "value"
      					region = "${var.AWS_REGION}"
     				}
			}

		Restore:
			

Data Sources :
	restrict ip addresses

Template File 

Modules :
	module from git  ->  terraform get command will load the git module to local repo 
	module from local
	
	module “module_name” {
		source = “path either git or local”
	}

Commands :
	terraform show
	terraform graph
	terraform refresh


aws_lambda_event_source_mapping
