# Infrastructure as Code

## Task

Create a Launch Configuration for your application servers in order to deploy four servers, two located in each of your private subnets. The launch configuration will be used by an auto-scaling group.

Two vCPUs and at least 4GB of RAM recommended. The Operating System to be used is Ubuntu 18. So, choose an Instance size and Machine Image (AMI) that best fits this spec. Be sure to allocate at least 10GB of disk space.

Udagram communicates on the default `HTTP Port: 80`, so your servers will need this inbound port open since it will be used with the Load Balancer and the Load Balancer Health Check.

As for outbound, the servers will need unrestricted internet access to be able to download and update its software.

The load balancer should allow all public traffic `(0.0.0.0/0)` on `port 80` inbound, which is the default `HTTP port`. Outbound, it will only be using `port 80` to reach the internal servers.

The application needs to be deployed into private subnets with a Load Balancer located in a public subnet.

One of the output exports of the CloudFormation script should be the public URL of the LoadBalancer.

While the instances are in public subnets, the `SSH port open (port 22)` needs to be open for access, in case of troubleshooting the instances.

Log information for UserData scripts is located in this file: `cloud-init-output.log` under the folder: `/var/log`.

## Components included:

- S3 Read-only role
- VPC
- Subnets (2 x public, 2 x private)
- Internet Gateway attached to the VPC
- NAT Gateways with Elastic IPs (x2)
- Public Route Table
- Public Route associations (x2) between public subnets (x2) and the Public Route Table
- Private Route associations (x2) between private subnets (x2) and the Private Route Table
- Security Group for the VPC
- Launch Configuration
- Auto-scaling Group
- Load Balancer with its security group
- OUTPUTS: load balancer DNS Name


## To trigger stack creation:

``` aws cloudformation create-stack --stack-name Udagram --region eu-west-1 --template-body file://udagram.yml --parameters file://parameters.json --capabilities CAPABILITY_NAMED_IAM ```


## To trigger stack update:

``` aws cloudformation update-stack --stack-name Udagram --region eu-west-1 --template-body file://udagram.yml --parameters file://parameters.json --capabilities CAPABILITY_NAMED_IAM ```

## Possible improvements:

- deploy Windows Servers instead of Linux and use PowerShell scripts to showcase Windows management skills
- use AWS Parameter Store to save sensitive data, such as credentials to showcase attention to security
- use CloudWatch Alarms and CloudWatch custom metrics to showcase performance and monitoring skills