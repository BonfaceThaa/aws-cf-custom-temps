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

### Examples

```yaml
AWSTemplateFormatVersion: 2010-09-09
Description: AWS CloudFormation Sample Template VPC_EC2_Instance_With_Multiple_Static_IPAddresses.
Parameters:
  KeyName:
    Description: Name of an existing EC2 KeyPair to enable SSH access to the instance
    Type: 'AWS::EC2::KeyPair::KeyName'
    ConstraintDescription: must be the name of an existing EC2 KeyPair.
  InstanceType:
    Description: WebServer EC2 instance type
    Type: String
    Default: t2.small
    AllowedValues:
      - t1.micro
      - t2.nano
      - t2.micro
      - t2.small
      - t2.medium
      - t2.large
      - m1.small
      - m1.medium
      - m1.large
      - m1.xlarge
      - m2.xlarge
      - m2.2xlarge
      - m2.4xlarge
      - m3.medium
      - m3.large
      - m3.xlarge
      - m3.2xlarge
      - m4.large
      - m4.xlarge
      - m4.2xlarge
      - m4.4xlarge
      - m4.10xlarge
      - c1.medium
      - c1.xlarge
      - c3.large
      - c3.xlarge
      - c3.2xlarge
      - c3.4xlarge
      - c3.8xlarge
      - c4.large
      - c4.xlarge
      - c4.2xlarge
      - c4.4xlarge
      - c4.8xlarge
      - g2.2xlarge
      - g2.8xlarge
      - r3.large
      - r3.xlarge
      - r3.2xlarge
      - r3.4xlarge
      - r3.8xlarge
      - i2.xlarge
      - i2.2xlarge
      - i2.4xlarge
      - i2.8xlarge
      - d2.xlarge
      - d2.2xlarge
      - d2.4xlarge
      - d2.8xlarge
      - hi1.4xlarge
      - hs1.8xlarge
      - cr1.8xlarge
      - cc2.8xlarge
      - cg1.4xlarge
    ConstraintDescription: must be a valid EC2 instance type.
  VpcId:
    Type: 'AWS::EC2::VPC::Id'
    Description: VpcId of your existing Virtual Private Cloud (VPC)
    ConstraintDescription: must be the VPC Id of an existing Virtual Private Cloud.
  SubnetId:
    Type: 'AWS::EC2::Subnet::Id'
    Description: SubnetId of an existing subnet (for the primary network) in your VPC
    ConstraintDescription: must be an existing subnet in the selected Virtual Private Cloud.
  PrimaryIPAddress:
    Type: String
    Description: Primary private IP. This must be a valid IP address for Subnet
    AllowedPattern: '(\d{1,3})\.(\d{1,3})\.(\d{1,3})\.(\d{1,3})'
    ConstraintDescription: must be a valid IP address of the form x.x.x.x.
  SecondaryIPAddress:
    Type: String
    Description: Secondary private IP. This ust be a valid IP address for Subnet
    AllowedPattern: '(\d{1,3})\.(\d{1,3})\.(\d{1,3})\.(\d{1,3})'
    ConstraintDescription: must be a valid IP address of the form x.x.x.x.
  SSHLocation:
    Description: The IP address range that can be used to SSH to the EC2 instances
    Type: String
    MinLength: '9'
    MaxLength: '18'
    Default: 0.0.0.0/0
    AllowedPattern: '(\d{1,3})\.(\d{1,3})\.(\d{1,3})\.(\d{1,3})/(\d{1,2})'
    ConstraintDescription: must be a valid IP CIDR range of the form x.x.x.x/x.
Mappings:
  AWSInstanceType2Arch:
    t1.micro:
      Arch: HVM64
    t2.nano:
      Arch: HVM64
    t2.micro:
      Arch: HVM64
    t2.small:
      Arch: HVM64
    t2.medium:
      Arch: HVM64
    t2.large:
      Arch: HVM64
    m1.small:
      Arch: HVM64
    m1.medium:
      Arch: HVM64
    m1.large:
      Arch: HVM64
    m1.xlarge:
      Arch: HVM64
    m2.xlarge:
      Arch: HVM64
    m2.2xlarge:
      Arch: HVM64
    m2.4xlarge:
      Arch: HVM64
    m3.medium:
      Arch: HVM64
    m3.large:
      Arch: HVM64
    m3.xlarge:
      Arch: HVM64
    m3.2xlarge:
      Arch: HVM64
    m4.large:
      Arch: HVM64
    m4.xlarge:
      Arch: HVM64
    m4.2xlarge:
      Arch: HVM64
    m4.4xlarge:
      Arch: HVM64
    m4.10xlarge:
      Arch: HVM64
    c1.medium:
      Arch: HVM64
    c1.xlarge:
      Arch: HVM64
    c3.large:
      Arch: HVM64
    c3.xlarge:
      Arch: HVM64
    c3.2xlarge:
      Arch: HVM64
    c3.4xlarge:
      Arch: HVM64
    c3.8xlarge:
      Arch: HVM64
    c4.large:
      Arch: HVM64
    c4.xlarge:
      Arch: HVM64
    c4.2xlarge:
      Arch: HVM64
    c4.4xlarge:
      Arch: HVM64
    c4.8xlarge:
      Arch: HVM64
    g2.2xlarge:
      Arch: HVMG2
    g2.8xlarge:
      Arch: HVMG2
    r3.large:
      Arch: HVM64
    r3.xlarge:
      Arch: HVM64
    r3.2xlarge:
      Arch: HVM64
    r3.4xlarge:
      Arch: HVM64
    r3.8xlarge:
      Arch: HVM64
    i2.xlarge:
      Arch: HVM64
    i2.2xlarge:
      Arch: HVM64
    i2.4xlarge:
      Arch: HVM64
    i2.8xlarge:
      Arch: HVM64
    d2.xlarge:
      Arch: HVM64
    d2.2xlarge:
      Arch: HVM64
    d2.4xlarge:
      Arch: HVM64
    d2.8xlarge:
      Arch: HVM64
    hi1.4xlarge:
      Arch: HVM64
    hs1.8xlarge:
      Arch: HVM64
    cr1.8xlarge:
      Arch: HVM64
    cc2.8xlarge:
      Arch: HVM64
  AWSInstanceType2NATArch:
    t1.micro:
      Arch: NATHVM64
    t2.nano:
      Arch: NATHVM64
    t2.micro:
      Arch: NATHVM64
    t2.small:
      Arch: NATHVM64
    t2.medium:
      Arch: NATHVM64
    t2.large:
      Arch: NATHVM64
    m1.small:
      Arch: NATHVM64
    m1.medium:
      Arch: NATHVM64
    m1.large:
      Arch: NATHVM64
    m1.xlarge:
      Arch: NATHVM64
    m2.xlarge:
      Arch: NATHVM64
    m2.2xlarge:
      Arch: NATHVM64
    m2.4xlarge:
      Arch: NATHVM64
    m3.medium:
      Arch: NATHVM64
    m3.large:
      Arch: NATHVM64
    m3.xlarge:
      Arch: NATHVM64
    m3.2xlarge:
      Arch: NATHVM64
    m4.large:
      Arch: NATHVM64
    m4.xlarge:
      Arch: NATHVM64
    m4.2xlarge:
      Arch: NATHVM64
    m4.4xlarge:
      Arch: NATHVM64
    m4.10xlarge:
      Arch: NATHVM64
    c1.medium:
      Arch: NATHVM64
    c1.xlarge:
      Arch: NATHVM64
    c3.large:
      Arch: NATHVM64
    c3.xlarge:
      Arch: NATHVM64
    c3.2xlarge:
      Arch: NATHVM64
    c3.4xlarge:
      Arch: NATHVM64
    c3.8xlarge:
      Arch: NATHVM64
    c4.large:
      Arch: NATHVM64
    c4.xlarge:
      Arch: NATHVM64
    c4.2xlarge:
      Arch: NATHVM64
    c4.4xlarge:
      Arch: NATHVM64
    c4.8xlarge:
      Arch: NATHVM64
    g2.2xlarge:
      Arch: NATHVMG2
    g2.8xlarge:
      Arch: NATHVMG2
    r3.large:
      Arch: NATHVM64
    r3.xlarge:
      Arch: NATHVM64
    r3.2xlarge:
      Arch: NATHVM64
    r3.4xlarge:
      Arch: NATHVM64
    r3.8xlarge:
      Arch: NATHVM64
    i2.xlarge:
      Arch: NATHVM64
    i2.2xlarge:
      Arch: NATHVM64
    i2.4xlarge:
      Arch: NATHVM64
    i2.8xlarge:
      Arch: NATHVM64
    d2.xlarge:
      Arch: NATHVM64
    d2.2xlarge:
      Arch: NATHVM64
    d2.4xlarge:
      Arch: NATHVM64
    d2.8xlarge:
      Arch: NATHVM64
    hi1.4xlarge:
      Arch: NATHVM64
    hs1.8xlarge:
      Arch: NATHVM64
    cr1.8xlarge:
      Arch: NATHVM64
    cc2.8xlarge:
      Arch: NATHVM64
  AWSRegionArch2AMI:
    af-south-1:
      HVM64: ami-064cc455f8a1ef504
      HVMG2: NOT_SUPPORTED
    ap-east-1:
      HVM64: ami-f85b1989
      HVMG2: NOT_SUPPORTED
    ap-northeast-1:
      HVM64: ami-0b2c2a754d5b4da22
      HVMG2: ami-09d0e0e099ecabba2
    ap-northeast-2:
      HVM64: ami-0493ab99920f410fc
      HVMG2: NOT_SUPPORTED
    ap-northeast-3:
      HVM64: ami-01344f6f63a4decc1
      HVMG2: NOT_SUPPORTED
    ap-south-1:
      HVM64: ami-03cfb5e1fb4fac428
      HVMG2: ami-0244c1d42815af84a
    ap-southeast-1:
      HVM64: ami-0ba35dc9caf73d1c7
      HVMG2: ami-0e46ce0d6a87dc979
    ap-southeast-2:
      HVM64: ami-0ae99b503e8694028
      HVMG2: ami-0c0ab057a101d8ff2
    ca-central-1:
      HVM64: ami-0803e21a2ec22f953
      HVMG2: NOT_SUPPORTED
    cn-north-1:
      HVM64: ami-07a3f215cc90c889c
      HVMG2: NOT_SUPPORTED
    cn-northwest-1:
      HVM64: ami-0a3b3b10f714a0ff4
      HVMG2: NOT_SUPPORTED
    eu-central-1:
      HVM64: ami-0474863011a7d1541
      HVMG2: ami-0aa1822e3eb913a11
    eu-north-1:
      HVM64: ami-0de4b8910494dba0f
      HVMG2: ami-32d55b4c
    eu-south-1:
      HVM64: ami-08427144fe9ebdef6
      HVMG2: NOT_SUPPORTED
    eu-west-1:
      HVM64: ami-015232c01a82b847b
      HVMG2: ami-0d5299b1c6112c3c7
    eu-west-2:
      HVM64: ami-0765d48d7e15beb93
      HVMG2: NOT_SUPPORTED
    eu-west-3:
      HVM64: ami-0caf07637eda19d9c
      HVMG2: NOT_SUPPORTED
    me-south-1:
      HVM64: ami-0744743d80915b497
      HVMG2: NOT_SUPPORTED
    sa-east-1:
      HVM64: ami-0a52e8a6018e92bb0
      HVMG2: NOT_SUPPORTED
    us-east-1:
      HVM64: ami-032930428bf1abbff
      HVMG2: ami-0aeb704d503081ea6
    us-east-2:
      HVM64: ami-027cab9a7bf0155df
      HVMG2: NOT_SUPPORTED
    us-west-1:
      HVM64: ami-088c153f74339f34c
      HVMG2: ami-0a7fc72dc0e51aa77
    us-west-2:
      HVM64: ami-01fee56b22f308154
      HVMG2: ami-0fe84a5b4563d8f27
Resources:
  EIP1:
    Type: 'AWS::EC2::EIP'
    Properties:
      Domain: vpc
    Metadata:
      'AWS::CloudFormation::Designer':
        id: 8f31068a-6a3b-4aa6-918f-7c24edb0f98a
  EIPAssoc1:
    Type: 'AWS::EC2::EIPAssociation'
    Properties:
      NetworkInterfaceId: !Ref Eth0
      AllocationId: !GetAtt 
        - EIP1
        - AllocationId
    Metadata:
      'AWS::CloudFormation::Designer':
        id: f2488b5f-a2f4-4b46-a288-efd3b03258a5
  SSHSecurityGroup:
    Type: 'AWS::EC2::SecurityGroup'
    Properties:
      VpcId: !Ref VpcId
      GroupDescription: Enable SSH access via port 22
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: '22'
          ToPort: '22'
          CidrIp: !Ref SSHLocation
    Metadata:
      'AWS::CloudFormation::Designer':
        id: feae6bee-c293-48c4-9376-05df2be390eb
  EC2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      ImageId: !FindInMap 
        - AWSRegionArch2AMI
        - !Ref 'AWS::Region'
        - !FindInMap 
          - AWSInstanceType2Arch
          - !Ref InstanceType
          - Arch
      InstanceType: !Ref InstanceType
      KeyName: !Ref KeyName
      NetworkInterfaces:
        - NetworkInterfaceId: !Ref Eth0
          DeviceIndex: '0'
      Tags:
        - Key: Name
          Value: myInstance
    Metadata:
      'AWS::CloudFormation::Designer':
        id: 19b3e96e-bcde-418d-ad08-9dd6f14cda45
  Eth0:
    Type: 'AWS::EC2::NetworkInterface'
    Properties:
      Description: eth0
      GroupSet:
        - !Ref SSHSecurityGroup
      PrivateIpAddresses:
        - PrivateIpAddress: !Ref PrimaryIPAddress
          Primary: 'true'
        - PrivateIpAddress: !Ref SecondaryIPAddress
          Primary: 'false'
      SourceDestCheck: 'true'
      SubnetId: !Ref SubnetId
      Tags:
        - Key: Name
          Value: Interface 0
        - Key: Interface
          Value: eth0
    Metadata:
      'AWS::CloudFormation::Designer':
        id: 703aca2f-f7ac-476d-9431-9cf135a973c5
Outputs:
  InstanceId:
    Value: !Ref EC2Instance
    Description: Instance Id of newly created instance
  EIP1:
    Value: !Join 
      - ' '
      - - IP address
        - !Ref EIP1
        - on subnet
        - !Ref SubnetId
    Description: Primary public IP of Eth0
  PrimaryPrivateIPAddress:
    Value: !Join 
      - ' '
      - - IP address
        - !GetAtt 
          - Eth0
          - PrimaryPrivateIpAddress
        - on subnet
        - !Ref SubnetId
    Description: Primary private IP address of Eth0
  SecondaryPrivateIPAddresses:
    Value: !Join 
      - ' '
      - - IP address
        - !Select 
          - '0'
          - !GetAtt 
            - Eth0
            - SecondaryPrivateIpAddresses
        - on subnet
        - !Ref SubnetId
    Description: Secondary private IP address of Eth0
Metadata:
  'AWS::CloudFormation::Designer':
    feae6bee-c293-48c4-9376-05df2be390eb:
      size:
        width: 60
        height: 60
      position:
        x: 60
        'y': 90
      z: 1
      embeds: []
    703aca2f-f7ac-476d-9431-9cf135a973c5:
      size:
        width: 60
        height: 60
      position:
        x: 180
        'y': 90
      z: 1
      embeds: []
      isassociatedwith:
        - feae6bee-c293-48c4-9376-05df2be390eb
    19b3e96e-bcde-418d-ad08-9dd6f14cda45:
      size:
        width: 60
        height: 60
      position:
        x: 60
        'y': 210
      z: 1
      embeds: []
      isassociatedwith:
        - 703aca2f-f7ac-476d-9431-9cf135a973c5
    8f31068a-6a3b-4aa6-918f-7c24edb0f98a:
      size:
        width: 60
        height: 60
      position:
        x: 180
        'y': 210
      z: 1
      embeds: []
    f2488b5f-a2f4-4b46-a288-efd3b03258a5:
      source:
        id: 8f31068a-6a3b-4aa6-918f-7c24edb0f98a
      target:
        id: 703aca2f-f7ac-476d-9431-9cf135a973c5
```

## Return values

### Ref
When you pass the logical ID of this resource to the intrinsic Ref function, Ref returns the ID of the VPC.

### Fn::GetAtt
The Fn::GetAtt intrinsic function returns a value for a specified attribute of this type. 

## References
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpc.html
* https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/sample-templates-services-ap-south-1.html#w2ab1c35c30c13c37