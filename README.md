<h1>Build a VPC network from scratch and configure AWS Systems Manager to securely connect to EC2 instances without needing a bastion host and without needing SSH keys automated with Terraform.</h1>

<h2>Project details</h2>
In this project, we’re going to build a VPC network from scratch and configure AWS Systems Manager so that we can securely connect to EC2 instances without needing a bastion host and without needing SSH keys. 

<img width="511" height="281" alt="image" src="https://github.com/user-attachments/assets/250776d2-27eb-4681-a603-1b158d6222fd" />

<h1>Architecture</h1>
<h2>VPC Network</h2>

The first part we’re going to build is a network module that will automatically deploy a VPC to our specifications. It will allow us to define the CIDR block and Availability Zones and automatically create the necessary subnets, route tables, NAT Gateway, and Internet Gateway in order to make this a fully functional VPC in which we can deploy resources.

<img width="515" height="290" alt="image" src="https://github.com/user-attachments/assets/414ebabe-7fa8-464a-88dd-14d05bccd7a4" />

<h2>Systems Manager</h2>
AWS Systems Manager (SSM) is a service that offers quite a few capabilities, including the ability to securely connect to and interact with our EC2 instances without the need for SSH keys. The network requirements for SSM require either outbound internet access over port 443 or an SSM VPC endpoint. Technically, we don’t need the SSM VPC endpoint in our architecture since our instances have the required access via the NAT Gateway and Internet Gateway. However, the SSM VPC endpoint is a more secure option since once the user authenticates with the AWS SSM service, the traffic is routed through AWS’s private network rather than the public internet.
That’s why we will also be deploying an SSM VPC Endpoint.
SSM will get enabled through a regional configuration called Default Host Management Configuration (DHMC).
Also, an IAM Role with the necessary permissions will be created and leveraged by SSM. This is the IAM Role that gets assumed by the SSM service, allowing it to interact with EC2 instances.

<img width="532" height="298" alt="image" src="https://github.com/user-attachments/assets/6b5c896e-a9c2-4b1f-8dac-e1c6ec8af2d3" />

<h2>EC2 Instances</h2>
We’ll then deploy an EC2 running the latest version of Amazon Linux in both the public and private subnets of our VPC. For SSM to work, it requires an agent to be installed on EC2 instances and Amazon Linux comes with the agent pre-installed, making our lives a little bit easier.

<img width="518" height="292" alt="image" src="https://github.com/user-attachments/assets/9a708b31-ccb9-4fd1-8241-762504987e59" />

<h1>Building the Virtual Private Cloud module</h1>
It’s time to get started! Go ahead and create a brand new directory (I’ll name mine project-vpc), and within that directory, create the necessary files and directories for this project as outlined below. We’ll fill them in as we go along. 

To create a directory, open your terminal and run the following command.

     Mkdir project-vpc

Change directory;

    cd project-vpc
Create the files

    touch main.tf provider.tf variables.tf output.tf

Create a sub directory
     
     mkdir Modules

     cd Modules
Create three sub directory under the Modules directory
     
          mkdir ec2 ssm vpc
Create the various .tf files in the ssm, vpc and ec2 diretory

<img width="449" height="294" alt="image" src="https://github.com/user-attachments/assets/84ded38d-ccaa-4588-b61c-5d63d5605933" />

The directories for this project as outlined below. 

<img width="767" height="328" alt="image" src="https://github.com/user-attachments/assets/e1aace18-9277-4128-8024-d4a5c2479a0f" />

<h1>Task 1: VPC Module</h1>
The VPC module will be the first we create and deploy. This will build our network infrastructure and create the necessary subnets, route tables, NAT Gateway, and Internet Gateway. Everything else will be built on top of this module.
Let’s first define the module in our root main.tf (not the one within the modules directory) file. /main.tf








