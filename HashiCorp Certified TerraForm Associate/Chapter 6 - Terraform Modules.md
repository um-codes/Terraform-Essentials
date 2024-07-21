## Accessing and Using Terraform Modules 
---
#### Terraform Modules - 
- A module is a container for multiple resources that are used together.
- Every Terraform configuration has at least one module, called the **root** module, which consists of code files in your main working directory.
----
#### Accessing Terraform Modules - 
- Modules can be downloaded or referenced from: 
	- **Terraform Public Registry** 
	- **A Private Registry**
	- **Your Local System** 
- Modules are referenced using a **module** block. 
	- **`module`** = Reserved Keyword 
	- **`my-vpc-module`** = Module Name 
	- **`source = "./modules/vpc"`** = Module Source (Mandatory)
	- **`version = "0.0.5"`** = Module Version 
	- **`region = var.region`** = Input Parameter(s) for Module 
```HCL 
module "my-vpc-module" {
	source = "./modules/vpc"
	version = "0.0.5"
	region = var.region
}
```
- Other Allowed Parameters: **count**, **for_each**, **providers**, and **depends_on**. 
---
#### Using Terraform Modules - 
- Modules can optionally take input and provide outputs to plug back into your main code. 
- Accessing module outputs in your code: 
- **`module.my-vpc-module.subnet_id`** - Module Output
```HCL 
resource "aws_instance" "my-vpc-module" {
	...... # other arguments
	subnet_id = module.my-vpc-module.subnet_id
}
```
---
## Interacting with Terraform Module Inputs and Outputs 
---
#### Declaring Module in Code - 
- Module inputs are arbitrarily named parameters that you pass inside the **module** block. 
- These inputs can be used as variables inside the module code. 
- **`server-name = 'us-east-1'`** = Input parameter(s) for Module 
```HCL 
module "my-vpc-module" {
	source      = "./modules/vpc"
	server-name = 'us-east-1'
}
```
- **Inside Module:** `var.server-name`
---
#### Terraform Module Outputs - 
- The outputs declared inside Terraform module code can be fed back into the root module or your main code. 
- **Output Invocation Convention in Terraform Code** - `module.<name-of-module>.<name-of-output>`
---
#### Invoking Module Output In Main Code - 
- **Inside your Module Code** - 
```HCL 
output "ip_address" {
	value = aws_instance.private_ip
}
```
- **`module.my-vpc-module.ip_address`**