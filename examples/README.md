`String`
A literal string.

For example, users could specify "MyUserName".

`Number`
An integer or float. AWS CloudFormation validates the parameter value as a number; however, when you use the parameter elsewhere in your template (for example, by using the Ref intrinsic function), the parameter value becomes a string.

For example, users could specify "8888".

`List<Number>`
An array of integers or floats that are separated by commas. AWS CloudFormation validates the parameter value as numbers; however, when you use the parameter elsewhere in your template (for example, by using the Ref intrinsic function), the parameter value becomes a list of strings.

For example, users could specify "80,20", and a Ref would result in ["80","20"].

`CommaDelimitedList`
An array of literal strings that are separated by commas. The total number of strings should be one more than the total number of commas. Also, each member string is space trimmed.

For example, users could specify "test,dev,prod", and a Ref would result in ["test","dev","prod"].

`AWS-Specific Parameter Types`
AWS values such as Amazon EC2 key pair names and VPC IDs. For more information, see AWS-specific parameter types.

`SSM Parameter Types`
Parameters that correspond to existing parameters in Systems Manager Parameter Store. You specify a Systems Manager parameter key as the value of the SSM parameter, and AWS CloudFormation fetches the latest value from Parameter Store to use for the stack. For more information, see SSM parameter types.
