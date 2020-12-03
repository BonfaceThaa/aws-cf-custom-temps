# AWS CLI (AWS CloudFormation) command reference
This guide illustrates creating S3 resource with various resources, mappings and ouputs using AWS CloudFormation Using aws cli

## Creating AWS S3 bucket with a deletion policy
`aws cloudformation create-stack --stack-name testS3 --template-body file://Pubicly_Accessible_S3_deletion_policy.ymal --parameters ParameterKey=S3Bucket,ParameterValue="testS3"`
