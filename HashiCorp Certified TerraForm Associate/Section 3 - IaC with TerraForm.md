## What is the TerraForm WorkFlow? 
---
#### The Core TerraForm WorkFlow: Write -> Plan -> Apply 
- **Write** - Write your TerraForm Code 
	- This would generally start off with creating a GitHub repo as a common best practice 
- **Plan** - Review 
	- You'll continually add and review changes to code in your project 
- **Apply** - Deploy 
	- After one last review/plan, you'll be ready to provision real infrastructure
---
## TerraForm Init
#### Initializing the Working Directory
* `terraform init` - Initializes the working directory that contains your TerraForm code
	* **Downloads Ancillary Components** - Downloads modules and plugins 
	* **Sets up Backend** - Sets up the backend for storing Terraform state file, a mechanism by which Terraform tracks resources 
----
## Terraform Key Concepts: Plan, Apply, and Destroy 
---
#### Remembering the TerraForm Workflow: Write -> Plan -> Apply 
- **Write** - Write your Terraform Code 
	- This would generally start off with creating a GitHub repo as a common best practice 
- **Plan** - Review 
	- You'll continually add and review changes to your code in your project 
- **Apply** - Deploy 
	- After one last review/plan, you'll be ready to provision real infrastructure 

#### Reviewing Your TerraForm Code: `terraform plan`
- Reads the code and then creates and shows a "plan" of execution/deployment 
	- Note: This command does not deploy anything. Consider this a read-only command. 
- Allows the user to "review" the action plan before executing anything 
- At this stage, authentication credentials are used to connect to your infrastructure, if required. 

#### Deploying your TerraForm Code: `terraform apply`
- Deploys the instructions and statements in the code 
- Updates the deployment state tracking mechanism file, a.k.a. "state file"

#### Cleaning Up Your TerraForm Deployment: `terraform destroy`
- Looks at the recorded, stored state file created during deployment and destroys all resources created by your code. 
- Should be used with caution, as it is a non-reversible command. Take backups, and be sure that you want to delete infrastructure. 
---
## Resource Addressing in TerraForm: Understanding TerraForm Code
#### TerraForm Code Snippet (Configuring The Provider)
```HCL
provider "aws" {
	region = "us-east-1"
}
```

- `provider` - is a **reserved keyword**
- `aws`- is a **provider name**
- `region = "us-east-1"` - are configuration parameters

```HCL
provider "google" {
	credentails = file("credentails.json)
	project     = "my-gcp-project"
	region      = "us-west-1"
}
```

- The configuration parameters are a Built-in Function 

#### TerraForm Code Snippet 
```HCL
resource "aws_instance" "web" {
	ami           = "ami-a1b2c3d4"
	instance_type = "t2.micro"
}
```

`resource` - is a **reserved keyword**
`"aws_instance"` = is the resource provided by the TerraForm provider 
`"web"` - is the user-provided arbitrary resource name 
`ami = "ami-a1b2c3d4"` - is the resource config argument 
`instance_type = "t2.micro"` - is the resource config argument 
`aws_instance.web` - would be the resource address 

#### TerraForm Code Snippet (Data Source) 
```HCL 
data "aws_instance" "my-vm" {
	instance-id = "i-1234567890abcdef0"
}
```

`data` - is a **reserved keyword**
`"aws_instance"` - is the resource provided by TerraForm provider 
`"my-vm"` - is the user-provided arbitrary resource name 
`instance_id = "i-1234567890abcdef0"` - is the data source argument(s)
`data.aws_instance.my-vm` - is the resource address

#### TerraForm Code 
- Terraform executes code in files with the `.tf` extension 
- Initially, Terraform looks for providers in the **Terraform Providers Registry** (https://registry.terraform.io/browse/providers)


