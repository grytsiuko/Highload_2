1. Configure AWS CLI

`aws configure`

2. Create VPC

`aws ec2 create-vpc --cidr-block 10.10.0.0/18 --no-amazon-provided-ipv6-cidr-block  --query Vpc.VpcId  --output text`