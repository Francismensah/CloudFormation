# RDS MySQL 5.7 CloudFormation Template

This CloudFormation project automates the creation of a MySQL 5.7 RDS database instance within a specified VPC. The template allows for customization of various parameters to configure the RDS instance according to your requirements.

## Architecture Diagram

![Create Your RDS Instance in the Private Subnet](./rds-assets/5%20-%20Create%20Your%20RDS%20Instance%20in%20the%20Private%20Subnet.jpg)

## Steps

1. **Prepare VPC Stack**
   - Ensure you have an existing VPC stack that exports necessary values.

2. **Create Database Subnet Group**
   - A subnet group is created for the RDS instance using subnets from the VPC stack.
   - Subnet IDs:
     - `${ExportVpcStackName}-PrivateSubnet3`
     - `${ExportVpcStackName}-PrivateSubnet4`

3. **Create RDS Database Instance**
   - An RDS instance is created with the following configurable parameters:
     - **Instance Identifier**: Default is `mysql57db`.
     - **Database Name**: Default is `applicationdb`.
     - **Database User**: Default is `dbadmin`.
     - **Database Password**: Default is `database1407`.
     - **Storage Allocation**: Default is `20` GB.
     - **Instance Class**: Default is `db.t3.micro`.
     - **Backup Retention Period**: Default is `0` days.
     - **Multi-AZ Deployment Option**: Default is `false`.

## Parameters

### ExportVpcStackName

- **Description:** The name of the VPC stack that exports values.
- **Type:** String

### DatabaseInstanceIdentifier

- **Description:** Instance identifier name.
- **Type:** String
- **Default:** `mysql57db`
- **Allowed Pattern:** `[a-zA-Z][a-zA-Z0-9]*`
- **Constraint Description:** Must begin with a letter and contain only alphanumeric characters.

### DatabaseName

- **Description:** MySQL database name.
- **Type:** String
- **Default:** `applicationdb`
- **Allowed Pattern:** `[a-zA-Z][a-zA-Z0-9]*`
- **Constraint Description:** Must begin with a letter and contain only alphanumeric characters.

### DatabaseUser

- **Description:** Username for MySQL database access.
- **Type:** String
- **Default:** `dbadmin`
- **Allowed Pattern:** `[a-zA-Z][a-zA-Z0-9]*`
- **Constraint Description:** Must begin with a letter and contain only alphanumeric characters.
- **NoEcho:** true

### DatabasePassword

- **Description:** Password for MySQL database access.
- **Type:** String
- **Default:** `database1407`
- **Allowed Pattern:** `[a-zA-Z0-9]*`
- **Constraint Description:** Must contain only alphanumeric characters.
- **NoEcho:** true

### DatabaseBackupRetentionPeriod

- **Description:** The number of days for which automatic DB snapshots are retained.
- **Type:** Number
- **Default:** 0
- **Constraint Description:** Must be between 0 and 35 days.

### DatabaseAllocatedStorage

- **Description:** The size of the database (Gb).
- **Type:** Number
- **Default:** 20
- **Constraint Description:** Must be between 5 and 1024 Gb.

### DatabaseInstanceClass

- **Description:** The database instance type.
- **Type:** String
- **Default:** `db.t3.micro`
- **Allowed Values:** `db.t1.micro`, `db.t2.micro`, `db.t3.micro`, `db.m1.small`, `db.m1.medium`, `db.m1.large`
- **Constraint Description:** Must select a valid database instance type.

### MultiAZDatabase

- **Description:** Creates a Multi-AZ MySQL Amazon RDS database instance.
- **Type:** String
- **Default:** `false`
- **Allowed Values:** `true`, `false`
- **Constraint Description:** Must be either true or false.

## Outputs

The CloudFormation template provides the following outputs:

- **RDSInstanceIdentifier:** The ID of the created RDS instance.
- **RDSInstanceEndpoint:** The endpoint address of the RDS instance.
- **RDSInstancePort:** The port on which the RDS instance is listening.

These outputs can be used to connect to the database instance and for reference in other resources.

## Usage

1. Save the CloudFormation template to a file, e.g., `rds.yaml`.
2. In the AWS Management Console, navigate to the CloudFormation service.
3. Click "Create stack" and choose "With new resources (standard)".
4. Select "Upload a template file" and choose the `rds.yaml` file.
5. Fill in the required parameters, such as the VPC stack name, database name, and user credentials.
6. Review the stack details and create the stack.

Once the stack is created, you can use the output values to connect to the RDS instance.

## About

This CloudFormation project automates the creation of an RDS MySQL 5.7 database instance within a specified VPC.
