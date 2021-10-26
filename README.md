

0. Configure AWS CLI

`aws configure`



1. Create VPC

`VPC=$(aws ec2 create-vpc --cidr-block 10.10.0.0/18 --no-amazon-provided-ipv6-cidr-block --query Vpc.VpcId --output text)`



2. Create 3 Subnets

`SUBNET1=$(aws ec2 create-subnet --vpc-id="$VPC" --cidr-block 10.10.1.0/24 --query Subnet.SubnetId --output text)`

`SUBNET2=$(aws ec2 create-subnet --vpc-id="$VPC" --cidr-block 10.10.2.0/24 --query Subnet.SubnetId --output text)`

`SUBNET3=$(aws ec2 create-subnet --vpc-id="$VPC" --cidr-block 10.10.3.0/24 --query Subnet.SubnetId --output text)`



(2.5). Create Internet Gateway

`IGW=$(aws ec2 create-internet-gateway --query InternetGateway.InternetGatewayId --output text)`

Attach to VPC

`aws ec2 attach-internet-gateway --internet-gateway-id $IGW --vpc-id $VPC`



3. Create Security Group

`SG=$(aws ec2 create-security-group --group-name my-sg --description "My security group" --vpc-id $VPC --output text)`

Enable SSH

`aws ec2 authorize-security-group-ingress --group-id $SG --protocol tcp --port 22 --cidr 0.0.0.0/0`

`aws ec2 authorize-security-group-ingress --group-id $SG --protocol tcp --port 80 --cidr 0.0.0.0/0`

`aws ec2 authorize-security-group-ingress --group-id $SG --protocol tcp --port 443 --cidr 0.0.0.0/0`



3. Create Launch Configuration

`aws autoscaling create-launch-configuration --launch-configuration-name my-lc --image-id ami-058e6df85cfc7760b --instance-type t2.micro --security-groups $SG`



3. Create Load Balancer

`aws elb create-load-balancer --load-balancer-name my-lb --listeners "Protocol=HTTP,LoadBalancerPort=80,InstanceProtocol=HTTP,InstancePort=80" --subnets $SUBNET1`



3. Create AWS Autoscaling group (ASG)

`aws autoscaling create-auto-scaling-group --auto-scaling-group-name my-asg --launch-configuration-name my-lc --min-size 1 --max-size 2 --vpc-zone-identifier "$SUBNET1" --load-balancer-names my-lb`


