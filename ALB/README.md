# Application Load Balancer CloudFormation Template

This CloudFormation project automates the creation of an Application Load Balancer (ALB) with associated listeners and target groups. The template leverages an existing VPC and integrates with AWS Certificate Manager (ACM) for SSL/TLS certificates.

## Steps

1. **Prepare VPC Stack**
   - Ensure you have an existing VPC stack that exports the necessary values.

2. **Create Application Load Balancer**
   - An ALB is created with the following properties:
     - **Name:** `MyApplicationLoadBalancer`
     - **Security Groups:** Imported from the VPC stack.
     - **Subnets:** Imported from the VPC stack.

3. **Create Listener on Port 80**
   - A listener on port 80 is created to redirect HTTP traffic to HTTPS:
     - **Port:** 80
     - **Protocol:** HTTP
     - **Action:** Redirect to HTTPS on port 443 with status code 301.

4. **Create Listener on Port 443**
   - A listener on port 443 is created to handle HTTPS traffic:
     - **Port:** 443
     - **Protocol:** HTTPS
     - **Certificate:** Provided by ACM.
     - **Default Action:** Forward traffic to the target group.

5. **Create Target Group**
   - A target group for web servers is created with health check configurations:
     - **Name:** `MyWebServers`
     - **Port:** 80
     - **Protocol:** HTTP
     - **Health Check Path:** `/`
     - **Health Check Interval:** 10 seconds
     - **Health Check Timeout:** 5 seconds
     - **Healthy Threshold:** 2
     - **Unhealthy Threshold:** 5
     - **Matcher:** HTTP codes 200 and 302

## Parameters

### AcmCertificate

- **Description:** The ARN of the AWS Certificate Manager's Certificate.
- **Type:** String

### ExportVpcStackName

- **Description:** The name of the VPC stack that exports values.
- **Type:** String

## Outputs

The CloudFormation template provides the following outputs:

- **ALBTargetGroup:** The ARN of the created target group.
- **ApplicationLoadBalancerDnsName:** The DNS name of the created ALB.
- **ApplicationLoadBalancerZoneID:** The canonical hosted zone ID of the created ALB.

These outputs can be used to reference the ALB and target group in other
