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