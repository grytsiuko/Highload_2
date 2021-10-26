0. Configure AWS CLI

`aws configure`

1. Create VPC

`aws ec2 create-vpc --cidr-block 10.10.0.0/18 --no-amazon-provided-ipv6-cidr-block  --query Vpc.VpcId  --output text`

Save output

`VPC=<generated-id>`

2. Create Subnets

`aws ec2 create-subnet --vpc-id="$VPC" --cidr-block 10.10.1.0/24`
`aws ec2 create-subnet --vpc-id="$VPC" --cidr-block 10.10.2.0/24`
`aws ec2 create-subnet --vpc-id="$VPC" --cidr-block 10.10.3.0/24`

(2.5). Create Internet Gateway

`aws ec2 create-internet-gateway --query InternetGateway.InternetGatewayId --output text`

Save output

`IGW=<generated-id>`

Attach to VPC

`aws ec2 attach-internet-gateway --internet-gateway-id $IGW --vpc-id $VPC`
