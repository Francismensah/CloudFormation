# VPC with CloudFormation

This CloudFormation project automates the creation of a Virtual Private Cloud (VPC) with public and private subnets, an internet gateway, and security groups.

## Architecture Diagram

![VPC Architecture Diagram](./VPC%20with%20Public%20and%20Private%20Subnets.jpg)

## Steps

1. **Create VPC**: The template creates a VPC with the specified CIDR block.
2. **Create Internet Gateway**: An internet gateway is created and attached to the VPC.
3. **Attach the Internet Gateway to the VPC**: The internet gateway is attached to the VPC.
4. **Create Public Subnets**: Two public subnets are created, each in a different availability zone.
5. **Create Public Route Table**: A public route table is created.
6. **Add Public Route to the Public Route Table**: A default route is added to the public route table, directing all traffic (0.0.0.0/0) to the internet gateway.
7. **Associate the Public Subnets with the Public Route Table**: The public subnets are associated with the public route table.
8. **Create the Private Subnets**: Four private subnets are created, two in each availability zone.
9. **Create the Security Groups**: Three security groups are created:
   - **ALB Security Group**: Allows HTTP and HTTPS traffic from anywhere.
   - **SSH Security Group**: Allows SSH traffic from a specified CIDR block.
   - **WebServer Security Group**: Allows HTTP and HTTPS traffic from the ALB Security Group, and SSH traffic from the SSH Security Group.

## Outputs

The CloudFormation template provides the following outputs:

- **VPC**: The ID of the created VPC.
- **PublicSubnet1**, **PublicSubnet2**: The IDs of the public subnets.
- **PrivateSubnet1**, **PrivateSubnet2**, **PrivateSubnet3**, **PrivateSubnet4**: The IDs of the private subnets.
- **ALBSecurityGroup**, **SSHSecurityGroup**, **WebServerSecurityGroup**, **DataBaseSecurityGroup**: The IDs of the security groups.

These outputs can be used as references for other resources that need to be deployed within the VPC.

## Usage

1. Save the CloudFormation template to a file, e.g., `vpc.yaml`.
2. In the AWS Management Console, navigate to the CloudFormation service.
3. Click "Create stack" and choose "With new resources (standard)".
4. Select "Upload a template file" and choose the `vpc.yaml` file.
5. Fill in the required parameters, such as the VPC CIDR, subnet CIDRs, and SSH access CIDR.
6. Review the stack details and create the stack.

Once the stack is created, you can use the output values to deploy other resources within the VPC, such as EC2 instances, databases, or load balancers.
