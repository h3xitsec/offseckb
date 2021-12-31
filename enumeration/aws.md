# configure creds
```
aws configure --profile profileName
```

# show account associated with key
```
aws sts get-access-key-info --access-key-id accesskeyxxxxxx --profile profileName
```

# list s3 bucket
```
aws s3 ls s3://bucketname --profile profileName
```

# show access key username
```
aws sts get-caller-identity --profile profileName
```

# list ec2 instances
```
aws ec2 describe-instances --output text --profile profileName --region us-east-1
```

# list secretsmanager secret
```
aws secretsmanager list-secrets --profile profileName --region us-east-1   
```