# AWS-CLI-CREATE-A-BUCKET
#Example:

1) Create key pair
aws ec2 create-key-pair --key-name steven0330 --query 'KeyMaterial' --output text
aws ec2 describe-key-pairs

2) Get VPC-ID
aws ec2 describe-vpcs
-> vpc-id: vpc-0d6330xxxx

3) Create security group
aws ec2 create-security-group --group-name StevenSG0330 --description "Steven security group" --vpc-id vpc-0d6330xxxx
aws ec2 describe-security-groups
-> security-group-id: sg-04171b4a25e5bxxxx

#add rules to security group:
aws ec2 authorize-security-group-ingress --group-id sg-04171b4a25e5bxxxx --protocol tcp --port 22 --cidr 0.0.0.0/0
aws ec2 authorize-security-group-ingress --group-id sg-04171b4a25e5bxxxx --protocol tcp --port 80 --cidr 0.0.0.0/0

4) Create bucket
-> via website EC2 Instance: ami id: ami-0bf54ac1b628cf143
aws ec2 describe-subnets
-> subnetId: subnet-08eb934ceb99bxxxx
aws ec2 run-instances --image-id ami-0bf54ac1b628cf143 --count 2 --instance-type t2.micro --key-name steven0330 --security-group-ids sg-04171b4a25e5bxxxx --subnet-id subnet-08eb934ceb99bxxxx

#Terminate an instance
aws ec2 describe-instances --query "Reservations[].Instances[].InstanceId"
[
    "i-0452bb2f59024xxxx",
    "i-021c2ffeb765exxxx"
]

#login
ssh -i steven0330.pem ec3-user@35.111.xx.xx

#clean up
aws ec2 terminate-instances --instance-ids i-0452bb2f59024xxxx
aws ec2 delete-key-pair --key-name steven0330
aws ec2 describe-security-groups
aws ec2 delete-security-group --group-id sg-04171b4a25e5bxxxx
