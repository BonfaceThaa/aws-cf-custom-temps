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
|--------------------|----------------------------------------------------------------------------------------:|
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

## References
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/sample-templates-services-ap-south-1.html#w2ab1c35c30c13c25
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-rds-database-instance.html
