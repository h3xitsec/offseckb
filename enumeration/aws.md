# configure creds
```bash
aws configure --profile profileName
```

# show account associated with key
```bash
aws sts get-access-key-info --access-key-id accesskeyxxxxxx --profile profileName
```

# list s3 bucket
```bash
aws s3 ls s3://bucketname --profile profileName
```

# show access key username
```bash
aws sts get-caller-identity --profile profileName
```

# list ec2 instances
```bash
aws ec2 describe-instances --output text --profile profileName --region us-east-1
```

# list secretsmanager secret
```bash
aws secretsmanager list-secrets --profile profileName --region us-east-1   
```