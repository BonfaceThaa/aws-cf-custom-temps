# AWS CLI (AWS CloudFormation) command reference
This guide illustrates creating S3 resource with various resources, mappings and ouputs using AWS CloudFormation Using aws cli

## S3 overview

### Template
```yaml
Type: AWS::S3::Bucket
Properties: 
  AccelerateConfiguration: 
    AccelerateConfiguration
  AccessControl: String
  AnalyticsConfigurations: 
    - AnalyticsConfiguration
  BucketEncryption: 
    BucketEncryption
  BucketName: String
  CorsConfiguration: 
    CorsConfiguration
  IntelligentTieringConfigurations: 
    - IntelligentTieringConfiguration
  InventoryConfigurations: 
    - InventoryConfiguration
  LifecycleConfiguration: 
    LifecycleConfiguration
  LoggingConfiguration: 
    LoggingConfiguration
  MetricsConfigurations: 
    - MetricsConfiguration
  NotificationConfiguration: 
    NotificationConfiguration
  ObjectLockConfiguration: 
    ObjectLockConfiguration
  ObjectLockEnabled: Boolean
  OwnershipControls: 
    OwnershipControls
  PublicAccessBlockConfiguration: 
    PublicAccessBlockConfiguration
  ReplicationConfiguration: 
    ReplicationConfiguration
  Tags: 
    - Tag
  VersioningConfiguration: 
    VersioningConfiguration
  WebsiteConfiguration: 
    WebsiteConfiguration
```

### Properties
|Property name|Description|
|--------------------|-------------------------------------------------------------:|
|AccessControl       |A canned ACL that grants predefined permissions to the bucket.|
|WebsiteConfiguration|Information used to configure the bucket as a static website. |
|BucketName          |A name for the bucket.                                        |

## Return values

### Ref
Passing the logical ID, Ref will return bucketname
 
### Fn::GetAtt
Returns these attributes: Arn, DomainName, DualStockName, RegionalDomainName, WebsiteURL

## Creating AWS S3 bucket with a deletion policy

`aws cloudformation create-stack --stack-name testS3 --template-body file://Pubicly_Accessible_S3_deletion_policy.ymal --parameters ParameterKey=S3Bucket,ParameterValue="testS3"`

TODO
## Create S3 website

`aws cloudformation create-stack --stack-name testS3 --template-body file://`

## Associate a replication configuration IAM role with an S3 bucket
## Configure a static website with a routing rule
## Enable cross-origin resource sharing
## Manage the lifecycle for S3 objects
## Log access requests for a specific S3 bucket
## Receive S3 bucket notifications to an SNS topic
## Replicate objects and store them in another S3 bucket
## Specify analytics and inventory configurations for an S3 bucket

## Resources
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/sample-templates-services-ap-south-1.html
* Pubicly_Accessible_S3_deletion_policy.ymal --parameters ParameterKey=S3Bucket,ParameterValue="testS3"Pubicly_Accessible_S3_deletion_policy.ymal --parameters ParameterKey=S3Bucket,ParameterValue="testS3
