<h1>Build a VPC network from scratch and configure AWS Systems Manager to securely connect to EC2 instances without needing a bastion host and without needing SSH keys automated with Terraform.</h1>

<h2>Project details</h2>
In this project, we’re going to build a VPC network from scratch and configure AWS Systems Manager so that we can securely connect to EC2 instances without needing a bastion host and without needing SSH keys. 

<img width="511" height="281" alt="image" src="https://github.com/user-attachments/assets/250776d2-27eb-4681-a603-1b158d6222fd" />

<h1>Architecture</h1>
<h2>VPC Network</h2>
The first part we’re going to build is a network module that will automatically deploy a VPC to our specifications. It will allow us to define the CIDR block and Availability Zones and automatically create the necessary subnets, route tables, NAT Gateway, and Internet Gateway in order to make this a fully functional VPC in which we can deploy resources.

