# Identity and Access Management (IAM)
This hands-on will focus on AWS Identity and Access Management.

## What I did
- Create a custom managed policy.
- Create IAM user groups with permission policies.
- Create IAM users and assign users to groups.
- Use user groups to add users to a group.
- Explore policy permissions that users inherit from groups.
- Log in as users to test the user’s permissions.
- Modify a user’s permission to provide additional access.

## Scenario
I want to create 3 user-groups with policies attached to them.
- S3-Support: with Read-only access to Amazon S3.
- EC2-Support: with Read-only access to Amazon EC2.
- EC2-Admin: with View, start, stop to Amazon EC2 Instances.

## Creating custom managed policy for EC2-Admin user-group
I created the following policy to enable view, start, stop Amazon EC2 instances.
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Statement1",
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances",
        "ec2:StartInstances",
        "ec2:StopInstances"
      ],
      "Resource": "*",
      "Condition": {
        "StringLikeIfExists": {
          "ec2:Owner": "amazon",
          "ec2:InstanceType": [
            "*.nano",
            "*.micro"
          ]
        }
      }
    }
  ]
}
```
This policy allows actions to view, start, and stop instances.
```json
"Effect": "Allow",
"Action": [
  "ec2:DescribeInstances",
  "ec2:StartInstances",
  "ec2:StopInstances"
],
```
It allows to perform those actions on all EC2 instances that are nano or micro family only.
```json
"Resource": "*",
"Condition": {
  "StringLikeIfExists": {
    "ec2:Owner": "amazon",
    "ec2:InstanceType": [
      "*.nano",
      "*.micro"
    ]
  }
}
```
I named the policy EC2-Admin-Policy

![IAM-1](./screenshots/iam/iam-1.png)

### Creating S3-Support, EC2-Support, and EC2-Admin user groups with policies.
S3-Support has AmazonS3ReadOnlyAccess.

![IAM-2](./screenshots/iam/iam-2.png)

EC2-Support has AmazonEC2ReadOnlyAccess.

![IAM-3](./screenshots/iam/iam-3.png)

EC2-Admin has my custom managed policy EC2-Admin-Policy

![IAM-4](./screenshots/iam/iam-4.png)

### Creating IAM users
user-1 is assigned to S3-Support user group.

![IAM-5](./screenshots/iam/iam-5.png)

user-2 is assigned to EC2-Support user group.

![IAM-6](./screenshots/iam/iam-6.png)

user-3 is assigned to EC2-Admin user group.

![IAM-7](./screenshots/iam/iam-7.png)

All of them inherited their respective user group's policies.

### Testing permissions of IAM users
Let's try stopping an instance using user-2 and see what happens.

![IAM-9](./screenshots/iam/iam-9.png)

As we can see it didn't allow user-2 to stop the ec2 instance because it is not authorized, it can only read ec2 instances not stop it. Now let's try stopping the instance using user-3 and see what happens.

![IAM-10](./screenshots/iam/iam-10.png)

It successfully performed the operation because it is permitted to do so. Now let's try accessing an S3 bucket using user-3.

![IAM-11](./screenshots/iam/iam-11.png)

The user-3 cannot view Amazon S3 buckets because it doesn't have permissions to view S3 buckets.

### Modifying permissions to allow user-3 to have a read only access to AmazonS3
I added user-3 to the S3-Support user group and now it inherits the group's permission to AmazonS3ReadOnlyAccess

![IAM-8](./screenshots/iam/iam-8.png)

Now let's see if user-3 will be able to see Amazon S3 buckets now that it is on S3-Support user group.

![IAM-12](./screenshots/iam/iam-12.png)

And it did! Amazon S3 buckets are now visible to user-3.