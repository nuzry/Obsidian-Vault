#### What is Terraform?

Terraform allows you to develop infrastructure as code. Mostly used for infrastructure on AWS
([https://github.com/deveprowse/tac-course](https://github.com/deveprowse/tac-course))

![[Pasted image 20260712204259.png]]
#### What is Iac (Infrastructure as code)

 - IaC is the code used to provision resources including
     - VMs
     - Network infrastructure
     - Security Groups
     - Users
     - more..

    IaC gives consistency, Reliable and Repeatable
  
#### IaC core Principles

- Versioned infrastructure: Multiple version of code that should be under source control (git).
- Idempotence: Consistency, no matter how many time it is run (one .tf can run as much as you want).
- Self-describing infrastructure: The infrastructure is the code and can be understood by peeps easy.
**More**: Easily re-creatable systems, repeatable processes, disposable systems, design is always changing, generic modules.

#### Why use Terraform
- Its quick and efficient.
- Works with multiple clouds (azure, AWS, Google Cloud etc.)
- Easy to read (.tf uses HCL code to write but it does not allow user defined functions.)
- The use of state. (Track resources and changes that are done to the infrastructure for developers to track. )
- The use of version controlling.

#### Terraform basic commands

![[Pasted image 20260712231523.png]]

The above picture show the basic commands of the terraform.

#### Main Commands

#init - initializing terraform in a DIR.
#fmt - This formats the code (indentation)
#validate - Checking the configuration is valid.
#plan - show changes that need for the current changes.
#apply - create or update infrastructure (pushing).
#destroy - Destroy already created infrastructure.

#### How Terraform Works?

Terraform allows us to:
 1. Write the code for infrastructure creation.
 
Sample code:
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}
```
In the above lines of code it says terraform, which cloud provider is used for infrastructure creation with its source, the HCL(HashiCorp Configuration Language). with what version and stuff""

```
provider "aws" {
  region = "us-east-2"
}
```
In the above lines it says the in the AWS provider which region to be used

```
resource "aws_instance" "lesson_03" {
  ami           = "ami-0c7c4e3c6b4941f0f"
  instance_type = "t2.micro"
  # Note: Change this to t3.micro if you created your AWS account after July 15th, 2025. Then, you can take advantage of free tier hours.
  
   tags = {
    Name = "Lesson-03-AWS-Instance"
  }
}
```
Now we are trying to create a resource in AWS using "aws instance" as the name being "lesson_03" 
It has 2 values 
- **ami** => Refers to the os and software installed on the instance
- **instance_type** => defines the size of the hardware instance.

 2. Initialize terraform init
 3. Apply terraform apply

#### Terraform workflow
Terraform has ==3== steps for the workflow
#write - Code your terraform file in any DIR and initialize terraform init
#plan - Allows to verify the configuration is valid and review the Terraform plan.
#apply Build the infrastructure based on the Terraform configuration.

note: The terraform apply will make changes to the infrastructure and to the state file as well.

