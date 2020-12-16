# AWS CLI (AWS CloudFormation) command reference
This guide illustrates creating RDS resource with various resources, mappings and ouputs using AWS CloudFormation Using aws cli

## RDS overview

### Template
```yaml
Type: AWS::RDS::DBInstance
Properties: 
  AllocatedStorage: String
  AllowMajorVersionUpgrade: Boolean
  AssociatedRoles: 
    - DBInstanceRole
  AutoMinorVersionUpgrade: Boolean
  AvailabilityZone: String
  BackupRetentionPeriod: Integer
  CACertificateIdentifier: String
  CharacterSetName: String
  CopyTagsToSnapshot: Boolean
  DBClusterIdentifier: String
  DBInstanceClass: String
  DBInstanceIdentifier: String
  DBName: String
  DBParameterGroupName: String
  DBSecurityGroups: 
    - String
  DBSnapshotIdentifier: String
  DBSubnetGroupName: String
  DeleteAutomatedBackups: Boolean
  DeletionProtection: Boolean
  Domain: String
  DomainIAMRoleName: String
  EnableCloudwatchLogsExports: 
    - String
  EnableIAMDatabaseAuthentication: Boolean
  EnablePerformanceInsights: Boolean
  Engine: String
  EngineVersion: String
  Iops: Integer
  KmsKeyId: String
  LicenseModel: String
  MasterUsername: String
  MasterUserPassword: String
  MaxAllocatedStorage: Integer
  MonitoringInterval: Integer
  MonitoringRoleArn: String
  MultiAZ: Boolean
  OptionGroupName: String
  PerformanceInsightsKMSKeyId: String
  PerformanceInsightsRetentionPeriod: Integer
  Port: String
  PreferredBackupWindow: String
  PreferredMaintenanceWindow: String
  ProcessorFeatures: 
    - ProcessorFeature
  PromotionTier: Integer
  PubliclyAccessible: Boolean
  SourceDBInstanceIdentifier: String
  SourceRegion: String
  StorageEncrypted: Boolean
  StorageType: String
  Tags: 
    - Tag
  Timezone: String
  UseDefaultProcessorFeatures: Boolean
  VPCSecurityGroups: 
    - String
```

### Properties
|Property name       |Description                                                                              |
|--------------------|-----------------------------------------------------------------------------------------|
|AllocatedStorage    |The amount of storage (in gigabytes) to be initially allocated for the database instance.|
|DBInstanceClass     |The compute and memory capacity of the DB instance.                                      |
|Engine              |The name of the database engine that you want to use for this DB instance.               |
|Iops                |The number of I/O operations per second (IOPS) that the database provisions.             |
|MasterUsername      |The master user name for the DB instance.                                                |
|MasterUserPassword  |The password for the master user.                                                        |
|MultiAZ             |Specifies whether the database instance is a multiple Availability Zone deployment.      |
|VPCSecurityGroups   |A list of the VPC security group IDs to assign to the DB instance.                       |
|DBSecurityGroups    |A list of the DB security groups to assign to the DB instance.                           |
|EngineVersion       |The version number of the database engine to use.|
|DBParameterGroupName|The name of an existing DB parameter group or a reference to an AWS::RDS::DBParameterGroup resource created in the template.|

## Return values

### Ref
When you pass the logical ID of this resource to the intrinsic Ref function, Ref returns the DB instance name.

### Fn::GetAtt
The Fn::GetAtt intrinsic function returns a value for a specified attribute of this type. The following are the available attributes and sample return values.

#### Endpoint.Address
The connection endpoint for the database.

#### Endpoint.Port
The port number on which the database accepts connections.

## 1. Create RDS instance with provisioned IOPS
Showing how to create an Amazon RDS Database Instance with provisioned IOPs.

`aws cloudformation create-stack --stack-name Dbtest1 --template-body file://RDS_Instance_with_IOPS.yaml --parameters ParameterKey=DBUser,ParameterValue={user} ParameterKey=DBPassword,ParameterValue={password}`

### Example
```yaml
AWSTemplateFormatVersion: 2010-09-09
Description: Create an Amazon RDS Database Instance with IOPS.
Parameters:
  DBUser:
    NoEcho: 'true'
    Description: The database admin account username
    Type: String
    MinLength: '1'
    MaxLength: '16'
    AllowedPattern: '[a-zA-Z][a-zA-Z0-9]*'
    ConstraintDescription: must begin with a letter and contain only alphanumeric characters.
  DBPassword:
    NoEcho: 'true'
    Description: The database admin account password
    Type: String
    MinLength: '8'
    MaxLength: '41'
    AllowedPattern: '[a-zA-Z0-9]*'
    ConstraintDescription: must contain only alphanumeric characters.
Resources:
  myDB:
    Type: 'AWS::RDS::DBInstance'
    Properties:
      AllocatedStorage: '100'
      DBInstanceClass: db.t2.small
      Engine: MySQL
      Iops: '1000'
      MasterUsername: !Ref DBUser
      MasterUserPassword: !Ref DBPassword
```

## 2. Create RDS DB instance with a read replica

`aws cloudformation create-stack --stack-name Dbtest1 --template-body file://RDS_instance_with_read_replica.yaml --parameters ParameterKey=DBUser,ParameterValue={user} ParameterKey=DBPassword,ParameterValue={password}`

### Template
```yaml
Type: AWS::RDS::DBSecurityGroup
Properties: 
  DBSecurityGroupIngress: 
    - Ingress
  EC2VpcId: String
  GroupDescription: String
  Tags: 
    - Tag
```

### Properties

|Property name         |Description                                          |
|----------------------|-----------------------------------------------------|
|DBSecurityGroupIngress|Ingress rules to be applied to the DB security group.|
|EC2VpcId              |The identifier of an Amazon VPC.                     |
|GroupDescription      |Provides the description of the DB security group.   |

### Example
```yaml
AWSTemplateFormatVersion: 2010-09-09
Description: Sample template showing how to create a highly-available, RDS DBInstance with a read replica.
Parameters:
  DBName:
    Default: MyDatabase
    Description: The database name
    Type: String
    MinLength: '1'
    MaxLength: '64'
    AllowedPattern: '[a-zA-Z][a-zA-Z0-9]*'
    ConstraintDescription: must begin with a letter and contain only alphanumeric characters.
  DBUser:
    NoEcho: 'true'
    Description: The database admin account username
    Type: String
    MinLength: '1'
    MaxLength: '16'
    AllowedPattern: '[a-zA-Z][a-zA-Z0-9]*'
    ConstraintDescription: must begin with a letter and contain only alphanumeric characters.
  DBPassword:
    NoEcho: 'true'
    Description: The database admin account password
    Type: String
    MinLength: '1'
    MaxLength: '41'
    AllowedPattern: '[a-zA-Z0-9]+'
    ConstraintDescription: must contain only alphanumeric characters.
  DBAllocatedStorage:
    Default: '5'
    Description: The size of the database (Gb)
    Type: Number
    MinValue: '5'
    MaxValue: '1024'
    ConstraintDescription: must be between 5 and 1024Gb.
  DBInstanceClass:
    Description: The database instance type
    Type: String
    Default: db.t2.small
    AllowedValues:
      - db.t1.micro
      - db.m1.small
      - db.m1.medium
      - db.m1.large
      - db.m1.xlarge
      - db.m2.xlarge
      - db.m2.2xlarge
      - db.m2.4xlarge
      - db.m3.medium
      - db.m3.large
      - db.m3.xlarge
      - db.m3.2xlarge
      - db.m4.large
      - db.m4.xlarge
      - db.m4.2xlarge
      - db.m4.4xlarge
      - db.m4.10xlarge
      - db.r3.large
      - db.r3.xlarge
      - db.r3.2xlarge
      - db.r3.4xlarge
      - db.r3.8xlarge
      - db.m2.xlarge
      - db.m2.2xlarge
      - db.m2.4xlarge
      - db.cr1.8xlarge
      - db.t2.micro
      - db.t2.small
      - db.t2.medium
      - db.t2.large
    ConstraintDescription: must select a valid database instance type.
  EC2SecurityGroup:
    Description: The EC2 security group that contains instances that need access to the database.
    Default: default
    Type: String
    AllowedPattern: '[a-zA-Z0-9\-]+'
    ConstraintDescription: must be a valid security group name.
  MultiAZ:
    Description: Multi-AZ master database
    Type: String
    Default: 'false'
    AllowedValues:
      - 'true'
      - 'false'
    ConstraintDescription: must be true or false.
Conditions:
  Is-EC2-VPC: !Or 
    - !Equals 
      - !Ref 'AWS::Region'
      - eu-central-1
    - !Equals 
      - !Ref 'AWS::Region'
      - cn-north-1
  Is-EC2-Classic: !Not 
    - !Condition Is-EC2-VPC
Resources:
  DBEC2SecurityGroup:
    Type: 'AWS::EC2::SecurityGroup'
    Condition: Is-EC2-VPC
    Properties:
      GroupDescription: Open database for access
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: '3306'
          ToPort: '3306'
          SourceSecurityGroupName: !Ref EC2SecurityGroup
  DBSecurityGroup:
    Type: 'AWS::RDS::DBSecurityGroup'
    Condition: Is-EC2-Classic
    Properties:
      DBSecurityGroupIngress:
        EC2SecurityGroupName: !Ref EC2SecurityGroup
      GroupDescription: database access
  MasterDB:
    Type: 'AWS::RDS::DBInstance'
    Properties:
      DBName: !Ref DBName
      AllocatedStorage: !Ref DBAllocatedStorage
      DBInstanceClass: !Ref DBInstanceClass
      Engine: MySQL
      MasterUsername: !Ref DBUser
      MasterUserPassword: !Ref DBPassword
      MultiAZ: !Ref MultiAZ
      Tags:
        - Key: Name
          Value: Master Database
      VPCSecurityGroups: !If 
        - Is-EC2-VPC
        - - !GetAtt 
            - DBEC2SecurityGroup
            - GroupId
        - !Ref 'AWS::NoValue'
      DBSecurityGroups: !If 
        - Is-EC2-Classic
        - - !Ref DBSecurityGroup
        - !Ref 'AWS::NoValue'
    DeletionPolicy: Snapshot
  ReplicaDB:
    Type: 'AWS::RDS::DBInstance'
    Properties:
      SourceDBInstanceIdentifier: !Ref MasterDB
      DBInstanceClass: !Ref DBInstanceClass
      Tags:
        - Key: Name
          Value: Read Replica Database
Outputs:
  EC2Platform:
    Description: Platform in which this stack is deployed
    Value: !If 
      - Is-EC2-VPC
      - EC2-VPC
      - EC2-Classic
  MasterJDBCConnectionString:
    Description: JDBC connection string for the master database
    Value: !Join 
      - ''
      - - 'jdbc:mysql://'
        - !GetAtt 
          - MasterDB
          - Endpoint.Address
        - ':'
        - !GetAtt 
          - MasterDB
          - Endpoint.Port
        - /
        - !Ref DBName
  ReplicaJDBCConnectionString:
    Description: JDBC connection string for the replica database
    Value: !Join 
      - ''
      - - 'jdbc:mysql://'
        - !GetAtt 
          - ReplicaDB
          - Endpoint.Address
        - ':'
        - !GetAtt 
          - ReplicaDB
          - Endpoint.Port
        - /
        - !Ref DBName
```
## 3. Create RDS DB instance with a deletion policy
Creates an Amazon RDS database instance with a deletion policy that specifies Amazon RDS to take a snapshot of the database before deleting it.

`aws cloudformation create-stack --stack-name Dbtest1 --template-body file://RDS_Instance_with_deletion_policy.yaml --parameters ParameterKey=DBUser,ParameterValue={user} ParameterKey=DBPassword,ParameterValue={password}`

### Example
```yaml
AWSTemplateFormatVersion: 2010-09-09
Description: AWS CloudFormation Sample Template RDS_Snapshot_On_Delete.
Resources:
  MyDB:
    Type: 'AWS::RDS::DBInstance'
    Properties:
      DBName: MyDatabase
      AllocatedStorage: '5'
      DBInstanceClass: db.t2.small
      Engine: MySQL
      MasterUsername: myName
      MasterUserPassword: myPassword
    DeletionPolicy: Snapshot
Outputs:
  JDBCConnectionString:
    Description: JDBC connection string for the database
    Value: !Join 
      - ''
      - - 'jdbc:mysql://'
        - !GetAtt 
          - MyDB
          - Endpoint.Address
        - ':'
        - !GetAtt 
          - MyDB
          - Endpoint.Port
        - /MyDatabase
```

## 4. Create RDS DB instance with a database parameter group
Creates an Amazon RDS database instance with a database parameter group.

`aws cloudformation create-stack --stack-name Dbtest1 --template-body file://RDS_instance_with_db_parameter_group.yaml --parameters ParameterKey=DBUser,ParameterValue={user} ParameterKey=DBPassword,ParameterValue={password}`

### Template
``
Type: AWS::RDS::DBParameterGroup
Properties: 
  Description: String
  Family: String
  Parameters: 
    Key : Value
  Tags: 
    - Tag

### Properties
|Property name         |Description                                                             |
|----------------------|------------------------------------------------------------------------|
|Description           |Provides the customer-specified description for this DB parameter group.|
|Family                |The DB parameter group family name.                                     |
|Paremeters            |An array of parameter names and values for the parameter update.        |

## Example
```yaml
AWSTemplateFormatVersion: 2010-09-09
Description: AWS CloudFormation Sample Template RDS_with_DBParameterGroup.
Parameters:
  DBName:
    Default: MyDatabase
    Description: The database name
    Type: String
    MinLength: '1'
    MaxLength: '64'
    AllowedPattern: '[a-zA-Z][a-zA-Z0-9]*'
    ConstraintDescription: must begin with a letter and contain only alphanumeric characters.
  DBUser:
    NoEcho: 'true'
    Description: The database admin account username
    Type: String
    MinLength: '1'
    MaxLength: '16'
    AllowedPattern: '[a-zA-Z][a-zA-Z0-9]*'
    ConstraintDescription: must begin with a letter and contain only alphanumeric characters.
  DBPassword:
    NoEcho: 'true'
    Description: The database admin account password
    Type: String
    MinLength: '8'
    MaxLength: '41'
    AllowedPattern: '[a-zA-Z0-9]*'
    ConstraintDescription: must contain only alphanumeric characters.
Resources:
  MyDB:
    Type: 'AWS::RDS::DBInstance'
    Properties:
      DBName: !Ref DBName
      AllocatedStorage: '5'
      DBInstanceClass: db.t2.small
      Engine: MySQL
      EngineVersion: 5.6.19
      MasterUsername: !Ref DBUser
      MasterUserPassword: !Ref DBPassword
      DBParameterGroupName: !Ref MyRDSParamGroup
  MyRDSParamGroup:
    Type: 'AWS::RDS::DBParameterGroup'
    Properties:
      Family: MySQL5.6
      Description: CloudFormation Sample Database Parameter Group
      Parameters:
        autocommit: '1'
        general_log: '1'
        old_passwords: '0'
Outputs:
  JDBCConnectionString:
    Description: JDBC connection string for the database
    Value: !Join 
      - ''
      - - 'jdbc:mysql://'
        - !GetAtt 
          - MyDB
          - Endpoint.Address
        - ':'
        - !GetAtt 
          - MyDB
          - Endpoint.Port
        - /
        - !Ref DBName
```

## Return values

### Ref
When you pass the logical ID of this resource to the intrinsic Ref function, Ref returns the name of the DB parameter group.

## References
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/sample-templates-services-ap-south-1.html#w2ab1c35c30c13c25
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-rds-database-instance.html
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-rds-security-group.html
