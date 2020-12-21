# AWS CloudFormation reference

## VPC Instance overview

### Template
```yaml
myVPC:
  Type: AWS::EC2::VPC
  Properties:
    CidrBlock: 10.0.0.0/16
    EnableDnsSupport: 'false'
    EnableDnsHostnames: 'false'
    InstanceTenancy: dedicated
    Tags:
     - Key: foo
       Value: bar
```

### Properties

|Properties        |Description                                                           |
|------------------|----------------------------------------------------------------------|
|CidrBlock         |The primary IPv4 CIDR block for the VPC.                              |
|EnableDnsHostnames|Indicates whether the instances launched in the VPC get DNS hostnames.|
|EnableDnsSupport  |Indicates whether the DNS resolution is supported for the VPC.        |
|InstanceTenancy   |The allowed tenancy of instances launched into the VPC.               |

## Return values

### Ref
When you pass the logical ID of this resource to the intrinsic Ref function, Ref returns the ID of the VPC.

### Fn::GetAtt
The Fn::GetAtt intrinsic function returns a value for a specified attribute of this type.


### 1. Create EC2 instances with multiple static IP addresses

`aws cloudformation create-stack --stack-name ec2vpc5 --template-body file://EC2_instance_with_VPC.yaml --parameters ParameterKey=KeyName,ParameterValue=MyKeyPair  ParameterKey=VpcId,ParameterValue=vpc-400cf03d ParameterKey=SubnetId,ParameterValue=subnet-efc06cb0 ParameterKey=PrimaryIPAddress,ParameterValue=172.31.32.8 ParameterKey=SecondaryIPAddress,ParameterValue=172.31.32.9`

### Example
Access a complete cloudformation template [here](../templates/EC2/EC2_instance_with_VPC.yaml)

## 2. Create EC2 instance with multiple dynamic IP addresses in an Amazon VPC

`aws cloudformation create-stack --stack-name ec2vpc42 --template-body file://EC2_instance_with_VPC_dynamic_IPS.yaml --parameters ParameterKey=KeyName,ParameterValue=MyKeyPair  ParameterKey=VpcId,ParameterValue=vpc-400cf03d ParameterKey=SubnetId,ParameterValue=subnet-997a09d4 ParameterKey=SecondaryIPAddressCount,ParameterValue=1` 

## EIPAssociation Overview
Associates an Elastic IP address with an instance or a network interface. 

### Template
```yaml
Type: AWS::EC2::EIPAssociation
Properties: 
  AllocationId: String
  EIP: String
  InstanceId: String
  NetworkInterfaceId: String
  PrivateIpAddress: String
```

### Properties
|Properties|Description|
|------------------|----------------------------------------------------------|
|AllocationId      |[EC2-VPC] The allocation ID. This is required for EC2-VPC.|
|NetworkInterfaceId|[EC2-VPC] The ID of the network interface.                |
|PrivateIpAddress  |[EC2-VPC] The primary or secondary private IP address to associate with the Elastic IP address.|

### Return values
#### Ref
When you pass the logical ID of this resource to the intrinsic Ref function, Ref returns the resource name.

## NetworkInterface overview

### Template
```yaml
Type: AWS::EC2::NetworkInterface
Properties: 
  Description: String
  GroupSet: 
    - String
  InterfaceType: String
  Ipv6AddressCount: Integer
  Ipv6Addresses: 
    - InstanceIpv6Address
  PrivateIpAddress: String
  PrivateIpAddresses: 
    - PrivateIpAddressSpecification
  SecondaryPrivateIpAddressCount: Integer
  SourceDestCheck: Boolean
  SubnetId: String
  Tags: 
    - Tag
```

### Properties
|Properties|Description|
|-----------|----------|
|GroupSet|A list of security group IDs associated with this network interface.|
|SourceDestCheck|Indicates whether traffic to or from the instance is validated.|
  
### Example
Access a complete cloudformation template [here](../templates/EC2/EC2_instance_with_VPC_dynamic_IPS.yaml)

## 3. Create publicly accessible EC2 instance with Auto Scaling group

`aws cloudformation create-stack --stack-name ec2vpcag --template-body file://EC2_instance_with_VPC_autoscaling_group.yaml --parameters ParameterKey=KeyName,ParameterValue=MyKeyPair`

## VPC Subnet overview

### Template

```yaml
Type: AWS::EC2::Subnet
Properties: 
  AssignIpv6AddressOnCreation: Boolean
  AvailabilityZone: String
  CidrBlock: String
  Ipv6CidrBlock: String
  MapPublicIpOnLaunch: Boolean
  OutpostArn: String
  Tags: 
    - Tag
  VpcId: String
```

## Example
```yaml
mySubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId:
        Ref: myVPC
      CidrBlock: 10.0.0.0/24
      AvailabilityZone: "us-east-1a"
      Tags:
      - Key: foo
        Value: bar
```

## AutoScalingGroup overview

## Template
```yaml
Type: AWS::AutoScaling::AutoScalingGroup
Properties: 
  AutoScalingGroupName: String
  AvailabilityZones: 
    - String
  CapacityRebalance: Boolean
  Cooldown: String
  DesiredCapacity: String
  HealthCheckGracePeriod: Integer
  HealthCheckType: String
  InstanceId: String
  LaunchConfigurationName: String
  LaunchTemplate: 
    LaunchTemplateSpecification
  LifecycleHookSpecificationList: 
    - LifecycleHookSpecification
  LoadBalancerNames: 
    - String
  MaxInstanceLifetime: Integer
  MaxSize: String
  MetricsCollection: 
    - MetricsCollection
  MinSize: String
  MixedInstancesPolicy: 
    MixedInstancesPolicy
  NewInstancesProtectedFromScaleIn: Boolean
  NotificationConfigurations: 
    - NotificationConfiguration
  PlacementGroup: String
  ServiceLinkedRoleARN: String
  Tags: 
    - TagProperty
  TargetGroupARNs: 
    - String
  TerminationPolicies: 
    - String
  VPCZoneIdentifier: 
    - String
```

### Properties
|Properties|Description|
|---|--------|
|VPCZoneIdentifier|A list of subnet IDs for a VPC where instances in the Auto Scaling group can be created.|
|LaunchConfigurationName|The name of the launch configuration to use to launch instances.|
|DesiredCapacity|The desired capacity is the initial capacity of the Auto Scaling group at the time of its creation.|
|TargetGroupARNs|One or more ARN of load balancer target groups to associate with the Auto Scaling group.|

### Example
Access a complete cloudformation template [here](../templates/EC2/EC2_instance_with_VPC_autoscaling_group.yaml)

## 4. Create EC2 instance with VPC plus DNS and public IP addresses
### Internet Gateway overview

`aws cloudformation create-stack --stack-name dnstest1 --template-body file://EC2_instance_with_VPC_dynamic_IPS.yaml --parameters ParameterKey=KeyName,ParameterValue=MyKeyPair ParameterKey=VpcId,ParameterValue=vpc-400cf03d ParameterKey=SubnetId,ParameterValue=subnet-997a09d4`

### Template
````yaml
Type: AWS::EC2::EgressOnlyInternetGateway
Properties: 
  VpcId: String
````

### Properties
|Properties|Description                                                            |
|----------|-----------------------------------------------------------------------|
|VpcId     |The ID of the VPC for which to create the egress-only internet gateway.|

### Example
```yaml
myEgressOnlyInternetGateway: 
    Type: AWS::EC2::EgressOnlyInternetGateway
    Properties: 
        VpcId: vpc-1a2b3c4d
```

Access a complete cloudformation template  [here](../templates/EC2/EC2_instance_with_VPC_DNS_publicIP.yaml)

## 5. Create EC2 instance with autoscaling and load balancing website
Creates a load balancing, auto scaling sample website in an existing VPC.

## LaunchConfiguration overview

### Template
```yaml
Type: AWS::AutoScaling::LaunchConfiguration
Properties: 
  AssociatePublicIpAddress: Boolean
  BlockDeviceMappings: 
    - BlockDeviceMapping
  ClassicLinkVPCId: String
  ClassicLinkVPCSecurityGroups: 
    - String
  EbsOptimized: Boolean
  IamInstanceProfile: String
  ImageId: String
  InstanceId: String
  InstanceMonitoring: Boolean
  InstanceType: String
  KernelId: String
  KeyName: String
  LaunchConfigurationName: String
  MetadataOptions: 
    MetadataOptions
  PlacementTenancy: String
  RamDiskId: String
  SecurityGroups: 
    - String
  SpotPrice: String
  UserData: String
```

### Properties
|Properties|Description|
|------------------------|------------------------------------------------------------------------------------------------|
|AssociatePublicIpAddress|For Auto Scaling groups that are running in a VPC.                                              |
|ImageId                 | Provides the unique ID of the Amazon Machine Image (AMI) that was assigned during registration.|

### Example
Access a complete cloudformation template [here](../templates/EC2_instance_with_VPC_autoscaling_group_load_balancing.yaml)

## References
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpc.html
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/sample-templates-services-ap-south-1.html#w2ab1c35c30c13c37
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-eip-association.html
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-network-interface.html
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-as-group.html
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-egressonlyinternetgateway.html
