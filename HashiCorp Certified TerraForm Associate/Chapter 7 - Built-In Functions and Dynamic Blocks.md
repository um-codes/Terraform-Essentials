## Terraform Built-In Functions 
---
#### Built-In Functions - 
---
- Terraform comes pre-packaged with functions to help you transform and combine values. 
- User-defined functions are not allowed - only built-in ones. 
- General Syntax: `function_name (arg1,arg2, ..)`
- Built-In Functions are extremely useful in making Terraform code dynamic and flexible. 
```HCL 
variable "project-name" {
	type = string 
	default = "prod"
}

resource "aws_vpc" "my-vpc" {
	cidr_block = "10.0.0.0/16"
	tags = {
		Name = join ("-", ["terraform", var.project-name])
	}
}
```
- Name = `terraform-prod`
----
#### User Functions - 
----
- A huge array of useful functions can be found here: https://developer.hashicorp.com/terraform/language/functions
	- file 
	- max 
	- flatten 
---
## Terraform Type Constraints (Collections and Structural)
---
#### Type Constraints - Terraform Variables 
- Type constraints can be used to validate user input and provide resource arguments. 
- Type of Variables - 
	- Primitive 
		- String 
		- Number 
		- Bool 
	- Complex 
		- Collection 
		- Structural 
#### Complex Types - 
- A complex type groups multiple values into a single value 
- Complex types are represented by type constructors 
#### Complex Types - Collections 
- A collection type allows multiple values of *one* other type to be grouped together as a single value. 
- Such variables are instantiated by constructors, such as **list(type), map(type),** or **set(type)**, where type can be any primitive variable type. 
- Example - **`type = list(string)`**, defining a "list" of 'strings'.
```HCL 
variable "my-var" {
	type = list(string)
	default = ["ACG", "LA"]
}
```
#### Complex Types - Structural 
- A structural type allows multiple values of *several distinct types* to be grouped together as a single value. 
- Such variables are instantiated by constructors, such as **object(type), tuple(type), or set(type)**, where type can be any primitive variable type. 
- Example - **`type = object({ name = string age = number })`** - defining an object variable with named attributes, each with their own type. 
```HCL 
variable "my-var" {
	type = object({ name = string
		age = number
		})
}
```
#### Dynamic Types - The "Any" Constraint 
---
- The keyword '**any**' is a special construct that serves as a placeholder for a type yet to be decided. 
- Examples included **list(any)** and **map(any)**. 
- Terraform will intelligently attempt to find an actual single type to replace '**any**' at runtime. 
- Example - **`type = list(any)`** 
```HCL 
variable "my-var" {
	type = list(any)
	default = [5, 6, 7]
}
```
- Terraform will replace the '**any**' keyword with **number** type on the fly, since all values passed are **number** type. 
---
## Terraform Dynamic Blocks 
----
#### What are Dynamic Blocks? 
- Dynamically constructs repeatable nested configuration blocks inside Terraform resources. 
- Supported within the following block types: 
	- Resource 
	- Data
	- Provider 
	- Provisioner 
- Used to make your code look cleaner 
- Terraform code below is a normal snippet of Terraform code without dynamic block:
```HCL 
resource "aws_security_group" "my-sg" {
	name   = 'my-aws-security-group'
	vpc_id = aws_vpc.my-vpc.id
	ingress {
		from_port   = 22
		to_port     = 22
		protocol    = "tcp"
		cidr_blocks = ["1.2.3.4/32"]
	}
	ingress {
		... #more ingress rules
	}
}
```
- **Dynamic Block Example** - 
```HCL 
resource "aws_security_group" "my-sg" {
	name   = 'my-aws-security-group'
	vpc_id = aws_vpc.my-vpc.id
	dynamic "ingress" // The config block you're trying to replicate
		for_each = var.rules // Complex Variable to Iterate Over
		content {                                //   
			from_port.  = ingress.value["port"]  // The nested "content"
			to_port     = ingress.value["port"]  // block defines the body of 
			protocol    = ingress.value["proto"] // each generated block, 
			cidr_blocks = ingress.value["cidrs"] //	using the variable you provided
		}
	}
}
```
- **Dynamic Block - Complex Variable Example**
```HCL
variable "rules" {
	default = [
		{
			port        = 80            // 1st Item within the Complex
			proto       = "tcp"         // Variable Type
			cidr_blocks = ["0.0.0.0/0"] //
		},
		{
			port        = 22             // 2nd Item within the Complex 
			proto       = "tcp"          // Variable Type
			cidr_blocks = ["1.2.3.4/32"] //
		}
	]
}
```
#### How to Configure Dynamic Blocks -
- Dynamic blocks expect a complex variable type to iterate over. 
- It acts like a for loop and outputs a nested block for each element in your variable. 
#### Caution! 
- Be careful not to overuse dynamic blocks in your main code, as they can be hard to read and maintain. 
- Only use dynamic blocks when you need to hide detail in order to build a cleaner user interface when writing reusable modules. 