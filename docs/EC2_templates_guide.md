# AWS CLI (AWS CloudFormation) command reference for EC2
`aws cloudformation create-stack --stack-name EC2test --template-body file://EC2_instance_with_security_group.ymal --parameters ParameterKey=KeyName,ParameterValue=MyKeyPair`

`aws cloudformation create-stack --stack-name test2ec2 --template-body file://EC2_instance_with_security_group_and_ElasticIP.yaml --parameters ParameterKey=InstanceType,ParameterValue=t2.micro ParameterKey=KeyName,ParameterValue=MyKeyPair`
