## Terraform State Command 
---
#### Terraform State (State Matters a Lot!)
- Simply put, it maps real-world resources to Terraform configuration.
- By default, state is stored locally in a file called `terraform.tfstate`.
- Prior to any modification operation, Terraform refreshes the state file. 
- Resource dependency metadata is also tracked via the state file. 
- Helps boost deployment performance by caching resource attributes for subsequent use. 
----
#### Terraform State Command - Scenarios 
- Terraform State Command is a utility for manipulating and reading the Terraform State file 
- **Scenario:**
	- **Advanced State Management**
	- **Manually remove a resource from Terraform State file so that it's not managed by Terraform**
	- **Listing out tracked resources and their details (via state and list subcommands)**
- **Common Terraform State Commands**
	- `terraform state list` - **List out all resources tracked by the Terraform State file**
	- `terraform state rm` - **Delete a resource from the Terraform State file**
	- `terraform state show` - **Show details of a resource tracked in the Terraform State file**
---
## Local and Remote State Storage 
---
#### Local State Storage 
- Saves Terraform state locally on your system 
- Terraform default behavior 
#### Remote State Storage 
- Saves state to a remote data resource. Optional. Examples of Storage: AWS S3, Google Storage
- Allows sharing state file between distributed teams 
- Allows locking state so parallel executions don't coincide 
- Enables sharing "output" values with other Terraform configuration or code 