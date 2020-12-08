# AWS CloudFormation reference

## Specifying a security group for an Instance
  
`aws cloudformation create-stack --stack-name EC2test --template-body file://EC2_instance_with_security_group.ymal --parameters ParameterKey=KeyName,ParameterValue=MyKeyPair`

`
Type: AWS::EC2::SecurityGroup
Properties: 
  GroupDescription: String
  GroupName: String
  SecurityGroupEgress: 
    - Egress
  SecurityGroupIngress: 
    - Ingress
  Tags: 
    - Tag
  VpcId: String
`
### Properties
|Property name |Description                                |
|--------------|:-----------------------------------------:|
|GroupDescription|A description for the security group.|
|GroupName|The name of the security group           |
|SecurityGroupEgress|The outbound rules associated with the security group.(VPC Only)|
|SecurityGroupIngress|The inbound rules associated with the security group.|
|Tags|Any tags assigned to the security group|
|VpcId|The ID of the VPC for the security group.(VPC only)|

## Allocating an Amazon EC2 Elastic IP Using AWS::EC2::EIP
Specifies an Elastic IP (EIP) address and can, optionally, associate it with an Amazon EC2 instance.

`aws cloudformation create-stack --stack-name test2ec2 --template-body file://EC2_instance_with_security_group_and_ElasticIP.yaml --parameters ParameterKey=InstanceType,ParameterValue=t2.micro ParameterKey=KeyName,ParameterValue=MyKeyPair`

`
Type: AWS::EC2::EIP
Properties: 
  Domain: String
  InstanceId: String
  PublicIpv4Pool: String
  Tags: 
    - Tag
`
### properties

|Property name |Description                                |
|--------------|:-----------------------------------------:|
|Domain        |Indicates resource linked(VPC/EC2 Instance)|
|InstanceId    |ID of the instance                         |
|PublicIpv4Pool|ID of address pool                         |
|Tags          |tags assigned to the Elastic IP address    |

### Example
`
MyEIP:
  Type: AWS::EC2::EIP
  Properties:
    InstanceId: !Ref Logical name of an AWS::EC2::Instance resource
`

### References
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-eip.html
* https://docs.aws.amazon.com/cli/latest/reference/cloudformation/create-stack.html
