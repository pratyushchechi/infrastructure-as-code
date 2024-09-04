# Infrastructure as Code Projects

This repository contains examples of using Infrastructure as Code (IaC) to automate the deployment of AWS resources using different tools, including AWS CloudFormation.

## Project 1: EC2 Instance Deployment with CloudFormation

### Overview
This project demonstrates how to deploy an EC2 instance on AWS using AWS CloudFormation. The CloudFormation template is parameterized to accept values such as the AMI ID and SSH key name at deployment time, ensuring sensitive information is not hardcoded.

### Files
- **ec2-instance.yaml**: The CloudFormation template used to create the EC2 instance.
- **screenshots/**: A folder containing screenshots of the deployment process and successful creation of the EC2 instance.

### Prerequisites
Before deploying the CloudFormation stack, ensure you have the following:
- An active AWS account
- AWS CLI installed and configured
- AMI ID and SSH key name stored in AWS Systems Manager (SSM) Parameter Store

### Setup Instructions

#### 1. Store Parameters in SSM
Before deploying the CloudFormation stack, store the necessary parameters in AWS Systems Manager Parameter Store:

```bash
aws ssm put-parameter --name "AMI_ID" --value "<your-ami-id>" --type "String"
aws ssm put-parameter --name "SSH_KEY_NAME" --value "<your-ssh-key-name>" --type "String"

Replace <your-ami-id> and <your-ssh-key-name> with the appropriate values for your environment.

2. Deploy the Stack
Retrieve the parameters from SSM and deploy the CloudFormation stack using the following commands:

bash
Copy code
AMI_ID=$(aws ssm get-parameter --name "AMI_ID" --query "Parameter.Value" --output text)
SSH_KEY_NAME=$(aws ssm get-parameter --name "SSH_KEY_NAME" --query "Parameter.Value" --output text)

aws cloudformation create-stack --stack-name ec2-instance-stack --template-body file://ec2-instance.yaml --parameters ParameterKey=AmiId,ParameterValue=$AMI_ID ParameterKey=KeyName,ParameterValue=$SSH_KEY_NAME

3. Verify the Deployment
After the stack is created, verify that the EC2 instance is running by checking the AWS Management Console or by using the AWS CLI:


aws ec2 describe-instances --filters "Name=tag:Name,Values=MyFirstInstance"
Cleaning Up
To avoid incurring charges, delete the CloudFormation stack once you're done with it:



aws cloudformation delete-stack --stack-name ec2-instance-stack

Future Enhancements

Security Groups: Add a security group to control inbound and outbound traffic.

Elastic IP: Associate an Elastic IP with the EC2 instance.

Multi-Instance Deployments: Extend the template to deploy multiple instances across different availability zones.

Integration with Other Services: Explore integrating the EC2 instance with other AWS services like RDS, S3, or Lambda.

Screenshots:
Screenshots of the deployment process and successful creation of the EC2 instance can be found in the screenshots/ directory.