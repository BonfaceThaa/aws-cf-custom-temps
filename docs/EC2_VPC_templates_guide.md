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



## 2. Create EC2 instance with multiple dynamic IP addresses in an Amazon VPC

`aws cloudformation create-stack --stack-name ec2vpc42 --template-body file://EC2_instance_with_VPC_dynamic_IPS.yaml --parameters ParameterKey=KeyName,ParameterValue=MyKeyPair  ParameterKey=VpcId,ParameterValue=vpc-400cf03d ParameterKey=SubnetId,ParameterValue=subnet-997a09d4 ParameterKey=SecondaryIPAddressCount,ParameterValue=1` 

### Example
Refer to EC2_instance_with_VPC_dynamic_IPS.yaml

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

## Properties


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

## References
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpc.html
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/sample-templates-services-ap-south-1.html#w2ab1c35c30c13c37
