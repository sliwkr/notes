# AWS CLI snippet cookbook

## table of contents

- [EC2](#ec2)
- [IAM](#iam)

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

