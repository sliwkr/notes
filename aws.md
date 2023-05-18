# AWS CLI snippet cookbook

## table of contents

- [EC2](#ec2)
- [IAM](#iam)
- [CloudFormation](#CloudFormation)
- [S3 Bucket](#s3)
- [Parameter Store](#ssm)

## snippets

### EC2

#### Start, stop instance

```sh
aws ec2 start-instances --instance-ids <id>
aws ec2 stop-instances --instance-ids <id>
```

#### Get instance ip

```sh
aws ec2 describe-instances --instance-ids <id> --query 'Reservations [*].Instances [*].PublicIpAddress' --output text
```

#### Check instance status

```sh
aws ec2 describe-instance-status --instance-ids <id> <id> \
  | jq '.InstanceStatuses[] | .InstanceId, .InstanceState.Name'
```

#### List running instances and their IP's

```sh
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" | jq '.Reservations[].Instances[] | .InstanceId, .PrivateIpAddress'
```

#### Get ec2 details by Name tag

```sh
aws ec2 describe-instances --filters 'Name=tag:Name,Values=<instance-name>'
```

#### Create ECR registry
```sh
aws ecr create-repository --repository-name <name> --profile <aws_credentials_profile>
```

#### Tag docker image appropriately

```sh
docker tag sm_image:TAG <ecr-repository-name>/sm_image:TAG
```

#### Push image to ECR

```sh
docker push <ecr-repository-name>/sm_image:TAG
```

### IAM

#### Set maximum role duration

```sh
aws iam update-role --role-name therolename --max-session-duration 28800
```


### CloudFormation

#### Check stack deployment status

```sh
aws cloudformation describe-stacks --stack-name the-stack-name | jq .Stacks[].StackStatus
aws --region us-east-1 cloudformation describe-stacks --stack-name the-stack-name | jq .Stacks[].StackStatus
```

#### Deploy stack

```sh
aws --region eu-west-1 cloudformation deploy --template-file template.yaml --stack-name ew1-stack-name
```


### S3

#### Get / download object from a bucket

```sh
aws s3api get-object \
    --bucket bucket-name \
    --key folder-or-prefix/object.gz \
    output.gz
```

### SSM

#### Get parameter properties

```sh
$ aws ssm describe-parameters --parameter-filters "Key=Name,Values=PARAM_NAME"
{
    "Parameters": [
        {
            "Name": "PARAM_NAME",
            "Type": "String",
            "LastModifiedDate": 1684329692.873,
            "LastModifiedUser": "arn:aws:sts::ffffffffffff:assumed-role/fffffffffffffffff/ffffff",
            "Version": 1,
            "Tier": "Standard",
            "Policies": [],
            "DataType": "text"
        }
    ]
}
```

```sh
$ aws ssm get-parameters --names "PARAM_NAME"
{
    "Parameters": [
        {
            "Name": "PARAM_NAME",
            "Type": "String",
            "Value": "theparamvalue",
            "Version": 1,
            "LastModifiedDate": 1684329692.873,
            "ARN": "arn:aws:ssm:eu-west-1:ffffffffffff:parameter/PARAM_NAME",
            "DataType": "text"
        }
    ],
    "InvalidParameters": []
}
```
