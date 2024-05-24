# RDS MySQL 5.7 CloudFormation Template

This project provides an AWS CloudFormation template to create a MySQL 5.7 RDS database instance. The template allows for customization of various parameters to configure the RDS instance according to your requirements.

## Architecture Diagram

An architecture diagram is included in the root directory to provide a visual representation of the setup.

![Create Your RDS Instance in the Private Subnet](./5%20-%20Create%20Your%20RDS%20Instance%20in%20the%20Private%20Subnet.jpg)

## Template Overview

The CloudFormation template (`rds.yaml`) sets up a MySQL 5.7 RDS instance with customizable parameters for instance identifier, database name, user credentials, storage, instance class, and backup retention.

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

## Resources

### DatabaseSubnetGroup

- **Type:** `AWS::RDS::DBSubnetGroup`
- **Properties:**
  - **DBSubnetGroupDescription:** Subnet group for RDS database.
  - **SubnetIds:**
    - `Fn::ImportValue`: `${ExportVpcStackName}-PrivateSubnet3`
    - `Fn::ImportValue`: `${ExportVpcStackName}-PrivateSubnet4`
  - **Tags:**
    - **Key:** `Name`
    - **Value:** `database subnets`

### DatabaseInstance

- **Type:** `AWS::RDS::DBInstance`
- **Properties:**
  - **AllocatedStorage:** `!Ref DatabaseAllocatedStorage`
  - **AvailabilityZone:** `!Select [0, !GetAZs ""]`
  - **BackupRetentionPeriod:** `!Ref DatabaseBackupRetentionPeriod`
  - **DBInstanceClass:** `!Ref DatabaseInstanceClass`
  - **DBInstanceIdentifier:** `!Ref DatabaseInstanceIdentifier`
  - **DBName:** `!Ref DatabaseName`
  - **DBSubnetGroupName:** `!Ref DatabaseSubnetGroup`
  - **Engine:** `MySQL`
  - **EngineVersion:** `5.7.44`
  - **MasterUsername:** `!Ref DatabaseUser`
  - **MasterUserPassword:** `!Ref DatabasePassword`
  - **MultiAZ:** `!Ref MultiAZDatabase`
  - **VPCSecurityGroups:**
    - `Fn::ImportValue`: `${ExportVpcStackName}-DataBaseSecurityGroup`

## Usage

1. Ensure you have the necessary CloudFormation setup.
2. Prepare your VPC stack name and other parameter values.
3. Deploy the CloudFormation template.
