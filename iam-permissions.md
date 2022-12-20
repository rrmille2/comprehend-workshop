# IAM Permissions for running Amazon Forecast in SageMaker Studio or Notebook

- From the AWS Console, type IAM and go to the IAM Console
- Next Click Roles on the left hand side
- Then click your execution role for SageMaker Studio (AmazonSageMaker-ExecutionRole-XXXXXXX)
- Add the following policies to your execution role
    - AmazonComprehendFullAccess
    - AmazonTextractFullAccess

- Add an inline policy and name it "role-passthru"
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "arn:aws:iam::*:role/*"
        }
    ]
}
```
- Modify the Trust Relationship to look like the following
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "comprehend.amazonaws.com",
                    "sagemaker.amazonaws.com"
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```
- Now create a new Role to give Amazon Forecast access to your data in S3
  - Create a new Role, select Custom Trust Policy
  - Inside the empty braces add this string: 
```
"Service": [ "comprehend.amazonaws.com" ]
```
  - Attach the Policy: AmazonS3FullAccess
  - Provide a Role Name, such as 'myDataAccessRole'
  - and click the "Create Role" button