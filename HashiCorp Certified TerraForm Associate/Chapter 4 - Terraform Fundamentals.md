## Installing Terraform 
---
#### There are Two Methods of Installing Terraform - 
- **Method 1 -** Download, Unzip, and Use 
- **Method 2** - Setup Terraform Repository on Linux (Only)
---
## Terraform Providers 
---
- Providers are Terraform's way of **abstracting** integrations with **API control layer** of the infrastructure vendors. 
- Terraform, by default, looks for Providers in the **Terraform Providers Registry** - https://registry.terraform.io/browse/providers
- Providers are **plugins**. They are released on a separate rhythm from Terraform itself, and each provider has it's own series of version of numbers. 
- You can write your own custom providers as well! 
- Terraform finds and installs providers when initializing working directory (via **terraform init**). 
- As a best practice Providers should be pegged down to a specific version, so that any changes across provider version doesn't break your Terraform code. 
---
## Terraform State: The Concept
---
#### Why State Matters? - Resource Tracking!!!
- **A way for Terraform to keep tabs on what has been deployed**. 
- **Critical to Terraforms functionality**. 
- **Stored in flat files, by default named `terraform.tfstate`. **
- **Helps Terraform calculate deployment delta and create new deployment plans**
- **Never lose your Terraform State file!**
---
## Terraform Variables and Outputs 
---
```HCL
variable "my-var" {
	description = "My Test Variable"
	type        = string 
	default     = "Hello" 
}
```
- `variable` = **Reserved keyword**
- `my-var` = **User-Provided Variable Name**. 
- Everything in between the brackets is - **variable config arguments such as type of variable and it's default value**. 
- If you want to reference the variable, `variable "my-var"`, then you can reference it via `var.my-var`
---
## Variables in Terraform - Validation Feature (Optional)
---
**Example -**
```HCL
variable "my-var" {
	description = "My Test Varaible"
	type        = string 
	default     = "Hello"
	validation {
		condition     = length(var.my-var) > 4
		error_message = "The string must be more than 4 characters"	
	}
}
```

**Example -**
```HCL
variable "my-var" {
	description = "My Test Variable"
	type        = string 
	default     = "Hello"
	sensitive   = true
}
```
---
## Variable Types Constraints 
---
#### Base Types: 
- **String**
- **Number** 
- **Bool**
#### Complex Types: 
- **List**
- **Set**
- **Map**
- **Object**
- **Tuple**
#### Examples -
##### String Type:  
```HCL
variable "image_id" {
	type    = string
	default = "Hello"
}
```
##### List Type: 
```HCL
variable "availability_zone_names" {
	type    = list(string) 
	default = ["us-west-1a"]  
}
```
##### List of Objects: 
```HCL 
variable "docker_ports" {
	type = list(object({
		internal = number
		external = number
		protocol = string
	}))
	default = [
		{
			internal = 8300
			external = 8300
			protocol = "tcp"
		}
	]
}
```
----
## Terraform Output - Output Values 
---
```HCL
output "instance_ip" {
	description = "VM's Private IP"
	value       = aws_instance.my-vm.private_ip
}
```
- `output` - **Reserved Keyword**
- `instance_ip` - **User-Provided Variable Name**
- Everything in between the brackets is **variable config arguments such as variable description and value**
- ###### Output variable values are shown on the shell after running `terraform apply`. 
- ###### Output values are like return values that you want to track after a successful Terraform deployment. 
---
## Terraform Provisioners: When to Use Them
---
- Terraforms way of bootstrapping custom scripts, commands or actions 
- Can be run either locally (on the same system where Terraform commands are being issued from), or remotely on resources spun up through the Terraform deployment. 
- Within Terraform code, each individual resources can have its own "provisioner" defining the connection method (if required such as SSH or WinRM) and the actions/commands or script to execute. 
- There are 2 types of provisioners: "Creation-time" and "Destroy-time" provisioners which you can set to run when a resource is being created or destroyed.
- **Disclaimer** - Hashicorp recommends using Provisioners as a last resort and to try using inherent mechanisms within your infrastructure deployment to carry out custom tasks where possible. 
- Terraform cannot track changes to provisioners as they can take any independent action, hence they are not tracked by Terraform state files. 
#### Best Practices and Cautions When Using Providers 
- Provisioners are recommended for use when you want to invoke actions not covered by Terraforms declarative model. 
- If the command within a provisioner returns non-zero return code, it's considered failed and underlying resource is tainted. 
#### Terraform Provisioner Example -
```HCL
resource "null_resource" "dummy_resource" {
	provisioner "local-exec" {
		command = "echo '0' > status.txt"
	}
	provisioner "local-exec" {
		when    = destroy
		command = "echo '1' > status.txt"
	}
}
```

