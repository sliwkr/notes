# AWS CLI snippet cookbook

## table of contents

- [EC2](#ec2)
- [IAM](#iam)
- [CloudFormation](#CloudFormation)
- [S3 Bucket](#s3)
- [Parameter Store](#ssm)
- [Route53](#Route53)

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

Note: docker-credential-ecr-login can be configured as well - then, it's sufficient to login using an aws profile

```sh
aws ecr get-login --no-include-email --profile <aws-profile> # aws cliv1
aws ecr get-login-password --region region | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com # aws cliv2
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
aws cloudformation create-stack \
    --stack-name <stack-name> \
    --template-body file://cfn.yaml \
    --parameters ParameterKey=<foo-name>,ParameterValue=<foo-value> ParameterKey=<bar-name>,ParameterValue=<bar-value> \
    --profile <aws-cli-profile> \
    --capabilities CAPABILITY_IAM
```


### S3

#### List contents

```sh
aws s3 ls bucket-name/folder-name/
```

#### Get / download object from a bucket

```sh
aws s3api get-object \
    --bucket bucket-name \
    --key folder-or-prefix/object.gz \
    output.gz
```

#### Delete object from a bucket

```sh
aws s3 rm s3://bucket-name/object
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

### Route53

#### Get records of a given hosted zone

```sh
# list existing hosted zone names and Id's
aws route53 list-hosted-zones --query 'HostedZones[].[Name, Id]'

# get interesting values of the records
aws route53 list-resource-record-sets --hosted-zone-id "/hostedzone/hostedzoneId" --query 'ResourceRecordSets[] .[Type,Name,TTL, ResourceRecords[].Value]'
```
