# CloudFormation NAT Gateway Automation

This CloudFormation template automates the creation of NAT gateways in each public subnet of a Virtual Private Cloud (VPC) in AWS. NAT gateways enable instances in private subnets to connect to the internet or other AWS services, while preventing inbound traffic from reaching those instances.

## Architecture Diagram

![NAT Gateway Architecture Diagram](./2%20-%20Create%20Your%20NAT%20Gateway.jpg)


## Overview

This template creates NAT gateways in each public subnet of the specified VPC. It also associates the appropriate route tables to direct internet-bound traffic through the NAT gateways.

## Parameters

- **ExportVpcStackName**: The name of the VPC stack that exports values.

## Resources

### Elastic IP Addresses (EIPs)

Two Elastic IP Addresses (EIPs) are allocated, one for each NAT gateway.

### NAT Gateways

Two NAT gateways are created, one in each public subnet, using the allocated EIPs.

### Route Tables

Two private route tables are created, one for each set of private subnets.

### Routes

- Each private route table has a route added to direct internet-bound traffic to the respective NAT gateway.
- Private subnets are associated with their corresponding private route tables.

## Usage

1. Save the CloudFormation template to a file, e.g., `nat_gateway.yaml`.
2. In the AWS Management Console, navigate to the CloudFormation service.
3. Click "Create stack" and choose "With new resources (standard)".
4. Upload the `nat_gateway.yaml` file and fill in the required parameters.
5. Review the stack details and create the stack.

Once the stack is created, NAT gateways will be provisioned in the public subnets of the specified VPC, enabling outbound internet access for instances in the associated private subnets.