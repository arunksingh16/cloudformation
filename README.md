# cloudformation
Why IaC
- Scaling
- Version Management
- Accountability
- Reusability


### Best Practices 
- Divide stacks into smaller chunks
- Reuse standard templates
- Mapped variables
- conditions
- Nested stacks
- cross stack ref instead of nested stack but check limitations

### Important Functions -
- The `Fn::GetAtt` intrinsic function returns the value of an attribute from a resource in the template
- `Fn::Join` appends a set of values into a single value, separated by the specified delimiter. Please note `AWS::Partition` Partition is a group of AWS Region and Service objects.
```
!Join
  - ''
  - - 'arn:'
    - !Ref AWS::Partition
    - ':s3:::elasticbeanstalk-*-'
    - !Ref AWS::AccountId
# output - arn:aws:s3:::elasticbeanstalk-*-xxxyyyxxx
```
- The intrinsic function `Fn::Select` returns a single object from a list of objects by index.
```
!Select [ 0, !Ref DbSubnetIpBlocks ]
```
- Fn::Split intrinsic function, split a string into a list of string values so that you can select an element from the resulting string list.
- The intrinsic function `Fn::ImportValue` returns the value of an output exported by another stack. For each AWS account, Export names must be unique within a region. Use `Fn::Sub` substitutes variables in an input string with values that you specify. You can use AWS defined params `AWS::StackName` for usability.
#### exporting from one stack
```
Outputs:
  PublicSubnet:
    Description: The subnet ID to use for public web servers
    Value:
      Ref: PublicSubnet
    Export:
      Name:
        'Fn::Sub': '${AWS::StackName}-SubnetID'
```
#### Importing in another
```
Resources:
  WebServerInstance:
    Type: 'AWS::EC2::Instance'
    Properties:
      InstanceType: t2.micro
      ImageId: ami-a1b23456
      NetworkInterfaces:
          AssociatePublicIpAddress: 'true'
          DeviceIndex: '0'
          DeleteOnTermination: 'true'
          SubnetId: !ImportValue 
            'Fn::Sub': '${PublicSubnet}-SubnetID'
```


### CloudFormation Support for external resources 
Custom resources (CRs) are the resources that don't fall under the official support of CloudFormation. CRs can be declared in a long (AWS::CloudFormation::CustomResource) or short form (Custom::MyResourceName)

#### Use Macros

#### AWS SAM
The SAM template, on its own, is nothing but a macro template that will be processed during deployment by CloudFormation.
```
Transform: AWS::Serverless-2016-10-31
Resources:
  DynamoDb:
    Type: AWS::Serverless::SimpleTable
    Properties:
      TableName: myTable
      PrimaryKey:
        Name: id
        Type: string
```

#### AWS CDK


## TIPS
- QUERY AMI
```
aws ec2 describe-images --owners amazon --filters "Name=name,Values=amzn*" --query 'sort_by(Images, &CreationDate)[].Name'
```
https://aws.amazon.com/blogs/compute/query-for-the-latest-amazon-linux-ami-ids-using-aws-systems-manager-parameter-store/
