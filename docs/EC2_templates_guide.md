# AWS CloudFormation reference

## EC2 Instance overview

### Template
```yaml
Type: AWS::EC2::Instance
Properties: 
  AdditionalInfo: String
  Affinity: String
  AvailabilityZone: String
  BlockDeviceMappings: 
    - BlockDeviceMapping
  CpuOptions: 
    CpuOptions
  CreditSpecification: 
    CreditSpecification
  DisableApiTermination: Boolean
  EbsOptimized: Boolean
  ElasticGpuSpecifications: 
    - ElasticGpuSpecification
  ElasticInferenceAccelerators: 
    - ElasticInferenceAccelerator
  HibernationOptions: 
    HibernationOptions
  HostId: String
  HostResourceGroupArn: String
  IamInstanceProfile: String
  ImageId: String
  InstanceInitiatedShutdownBehavior: String
  InstanceType: String
  Ipv6AddressCount: Integer
  Ipv6Addresses: 
    - InstanceIpv6Address
  KernelId: String
  KeyName: String
  LaunchTemplate: 
    LaunchTemplateSpecification
  LicenseSpecifications: 
    - LicenseSpecification
  Monitoring: Boolean
  NetworkInterfaces: 
    - NetworkInterface
  PlacementGroupName: String
  PrivateIpAddress: String
  RamdiskId: String
  SecurityGroupIds: 
    - String
  SecurityGroups: 
    - String
  SourceDestCheck: Boolean
  SsmAssociations: 
    - SsmAssociation
  SubnetId: String
  Tags: 
    - Tag
  Tenancy: String
  UserData: String
  Volumes: 
    - Volume
```

### Properties
|Property name       |Description                                                     |
|--------------------|:--------------------------------------------------------------:|
|ImageId             |The ID of the AMI.                                              |
|InstanceType        |The instance type.                                              |
|KeyName             |The name of the key pair.                                       |
|BlockDeviceMappings |Defines the block devices to attach to the instance at launch.  |
|NetworkInterfaces   |The network interfaces to associate with the instance.          |


## 1. Specifying a security group for an Instance
  
`aws cloudformation create-stack --stack-name EC2test --template-body file://EC2_instance_with_security_group.ymal --parameters ParameterKey=KeyName,ParameterValue=MyKeyPair`

### Template
```yaml
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
```

### Properties
|Property name       |Description                                                     |
|--------------------|:--------------------------------------------------------------:|
|GroupDescription    |A description for the security group.                           |
|GroupName           |The name of the security group                                  |
|SecurityGroupEgress |The outbound rules associated with the security group.(VPC Only)|
|SecurityGroupIngress|The inbound rules associated with the security group.           |
|Tags                |Any tags assigned to the security group                         | 
|VpcId               |The ID of the VPC for the security group.(VPC only)             |

### Example
```yaml
InstanceSecurityGroup:
  Type: AWS::EC2::SecurityGroup
  Properties:
      GroupDescription: Allow http to client host
      VpcId:
         Ref: myVPC
      SecurityGroupIngress:
      - IpProtocol: tcp
        FromPort: 80
        ToPort: 80
        CidrIp: 0.0.0.0/0
      SecurityGroupEgress:
      - IpProtocol: tcp
        FromPort: 80
        ToPort: 80
        CidrIp: 0.0.0.0/0
```

## 2. Allocating an Amazon EC2 Elastic IP Using AWS::EC2::EIP

Specifies an Elastic IP (EIP) address and can, optionally, associate it with an Amazon EC2 instance.

`aws cloudformation create-stack --stack-name test2ec2 --template-body file://EC2_instance_with_security_group_and_ElasticIP.yaml --parameters ParameterKey=InstanceType,ParameterValue=t2.micro ParameterKey=KeyName,ParameterValue=MyKeyPair`

### Template
```yaml
Type: AWS::EC2::EIP
Properties: 
  Domain: String
  InstanceId: String
  PublicIpv4Pool: String
  Tags: 
    - Tag
```

### Properties

|Property name |Description                                |
|--------------|:-----------------------------------------:|
|Domain        |Indicates resource linked(VPC/EC2 Instance)|
|InstanceId    |ID of the instance                         |
|PublicIpv4Pool|ID of address pool                         |
|Tags          |tags assigned to the Elastic IP address    |

### Example
```yaml
MyEIP:
  Type: AWS::EC2::EIP
  Properties:
    InstanceId: !Ref Logical name of an AWS::EC2::Instance resource
```

## 3. Creating an EC2 instance with emphemeral drive

Creates an Amazon EC2 instance with an ephemeral drive by using a block device mapping.

`aws cloudformation create-stack --stack-name test2ec2 --template-body file://EC2_instance_with_security_group_and_ElasticIP.yaml --parameters ParameterKey=KeyName,ParameterValue=MyKeyPair`

### Example
```yaml
 MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: "ami-79fd7eee"
      KeyName: "testkey"
      BlockDeviceMappings:
      - DeviceName: "/dev/sdm"
        Ebs:
          VolumeType: "io1"
          Iops: "200"
          DeleteOnTermination: "false"
          VolumeSize: "20"
      - DeviceName: "/dev/sdk"
        NoDevice: {}
```

## 4. Create an EC2 instance with EBS Block Device Mapping

## Volume

Specifies an Amazon Elastic Block Store (Amazon EBS) volume

### Template
```yaml
Type: AWS::EC2::Volume
Properties: 
  AutoEnableIO: Boolean
  AvailabilityZone: String
  Encrypted: Boolean
  Iops: Integer
  KmsKeyId: String
  MultiAttachEnabled: Boolean
  OutpostArn: String
  Size: Integer
  SnapshotId: String
  Tags: 
    - Tag
  VolumeType: String
```

### Properties
|Property name    |Description                                         |
|-----------------|:---------------------------------------------------|
|Availability zone|The Availability Zone in which to create the volume.|
|Size             |The size of the volume, in GiBs                     |
|VolumeType       |The volume type                                     |

## VolumeAttachment

Attaches an Amazon EBS volume to a running instance and exposes it to the instance with the specified device name.

### Template
```ymal
Type: AWS::EC2::VolumeAttachment
Properties: 
  Device: String
  InstanceId: String
  VolumeId: String
```

## Properties
|Property name|Description                                         |
|-------------|:---------------------------------------------------|
|Device       |The device name (for example, /dev/sdh or xvdh).    |
|InstanceId   |The ID of the instance to which the volume attaches.|
|VolumeId     |The ID of the Amazon EBS volume.                    |

### Example
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'AWS in Action: chapter 9 (EBS)'
Parameters:
  KeyName:
    Description: Key Pair name
    Type: 'AWS::EC2::KeyPair::KeyName'
    Default: mykey
  VPC:
    Description: 'Just select the one and only default VPC'
    Type: 'AWS::EC2::VPC::Id'
  Subnet:
    Description: 'Just select one of the available subnets'
    Type: 'AWS::EC2::Subnet::Id'
  AttachVolume:
    Description: 'Should the volume be attached?'
    Type: String
    Default: 'yes'
    AllowedValues:
    - 'yes'
    - 'no'
Mappings:
  RegionMap:
    'ap-south-1':
      AMI: 'ami-2ed19c41'
    'eu-west-3':
      AMI: 'ami-c8a017b5'
    'eu-west-2':
      AMI: 'ami-e3051987'
    'eu-west-1':
      AMI: 'ami-760aaa0f'
    'ap-northeast-2':
      AMI: 'ami-fc862292'
    'ap-northeast-1':
      AMI: 'ami-2803ac4e'
    'sa-east-1':
      AMI: 'ami-1678037a'
    'ca-central-1':
      AMI: 'ami-ef3b838b'
    'ap-southeast-1':
      AMI: 'ami-dd7935be'
    'ap-southeast-2':
      AMI: 'ami-1a668878'
    'eu-central-1':
      AMI: 'ami-e28d098d'
    'us-east-1':
      AMI: 'ami-6057e21a'
    'us-east-2':
      AMI: 'ami-aa1b34cf'
    'us-west-1':
      AMI: 'ami-1a033c7a'
    'us-west-2':
      AMI: 'ami-32d8124a'
Conditions:
  Attached: !Equals [!Ref AttachVolume, 'yes']
Resources:
  SecurityGroup:
    Type: 'AWS::EC2::SecurityGroup'
    Properties:
      GroupDescription: 'Allow incoming SSH from anywhere'
      VpcId: !Ref VPC
      SecurityGroupIngress:
      - CidrIp: '0.0.0.0/0'
        FromPort: 22
        ToPort: 22
        IpProtocol: tcp
      Tags:
      - Key: Name
        Value: 'AWS in Action: chapter 9 (EBS)'
  IamRole:
    Type: 'AWS::IAM::Role'
    Properties:
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
        - Effect: Allow
          Principal:
            Service: 'ec2.amazonaws.com'
          Action: 'sts:AssumeRole'
      Policies:
      - PolicyName: ec2
        PolicyDocument:
          Version: '2012-10-17'
          Statement:
          - Effect: Allow
            Action:
            - 'ec2:DescribeVolumes'
            - 'ec2:CreateSnapshot'
            - 'ec2:DescribeSnapshots'
            - 'ec2:DeleteSnapshot'
            Resource: '*'
  IamInstanceProfile:
    Type: AWS::IAM::InstanceProfile
    Properties:
      Roles:
      - !Ref IamRole
  EC2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      IamInstanceProfile: !Ref IamInstanceProfile
      ImageId: !FindInMap [RegionMap, !Ref 'AWS::Region', AMI]
      InstanceType: 't2.micro'
      KeyName: !Ref KeyName
      SecurityGroupIds:
      - !Ref SecurityGroup
      SubnetId: !Ref Subnet
      Tags:
      - Key: Name
        Value: 'AWS EBS example'
  Volume:
    Type: 'AWS::EC2::Volume'
    Properties:
      AvailabilityZone: !Sub ${EC2Instance.AvailabilityZone}
      Size: 5
      VolumeType: gp2
  VolumeAttachment:
    Type: 'AWS::EC2::VolumeAttachment'
    Condition: Attached
    Properties:
      Device: '/dev/xvdf'
      InstanceId: !Ref EC2Instance
      VolumeId: !Ref Volume
Outputs:
  PublicName:
    Value: !Sub ${EC2Instance.PublicDnsName}
    Description: 'Public name (connect via SSH as user ec2-user)'
  VolumeId:
    Value: !Ref Volume
    Description: 'ID of the EBS volume'
```

### References
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-eip.html
* https://docs.aws.amazon.com/cli/latest/reference/cloudformation/create-stack.html
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance.html
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-security-group.html
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-ebs-volume.html
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-ebs-volumeattachment.html
