# AWS Real-time Troubleshooting Session issue#01
## Start and Stop AWS EC2 Instances using 

Python Boto3 + Lambda
Project roadmap: 
	Create IAM Policy in AWS
	Create IAM Role and Attach Permission Policies
	Start and Stop AWS EC2 Instance using Python Boto3
	Create Lambda Function to Stop EC2
	Deploy Lambda function > Test > create new event
Step #1: Creating IAM Policy in AWS:
We have to create IAM Policy and Role which contains execution permission to EC2 instance and cloudwatch which we have to attach to Lambda function
To create IAM policy and Role Login to AWS Management console and search iam in search box.
You will redirected to IAM dashboard, click on Policies at left side.
Click on Create Policy
Select JSON and paste the below policy into it and click on Tags.
Add tags if you want and Click on Review
Give Name and Description to IAM Policy and click on Create Policy.
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:*:*:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:StartInstances",
                "ec2:StopInstances"
            ],
            "Resource": "arn:aws:ec2:region:account-id:instance/*"
        }
    ]
}
Step #2: Creating IAM Role and Attach Permission Policies:
Next We have to create role and attaching policy which we have created above to it.
To create role navigate to IAM and click on Roles on left side and click on Create Role.
Select AWS service, select Lambda and click on Permissions.
Attach existing policy which created above and click on Tags.
Give the tag.
Enter Role name and Role Description: allow lambda to start and stop ec2,  and click on Create Role.
Step #3: Start and Stop AWS EC2 Instance using Python Boto3:
Below are steps to Start and Stop AWS EC2 Instance using Python Boto3
Step #3.1: Lambda Function to Stop EC2 Instance Next Search Lambda and click on Create function.
In Author from scratch section give function name, Select Runtime as Python 3.8.
Under Permissions section, select use an existing role, in existing role add role which we have create above and click on Create function.
Copy the below Python Lambda function code to stop EC2 Instance, change EC2 instance name and region according to your and click on deploy: import boto3
Replace the ec2 instance ID
import boto3

def lambda_handler(event, context):
    region = 'us-east-1'
    instances = ['i-04306e1620ffcba9d', 'i-0331820f4b156914a', 'i-0ee6f5f1f9032c0d8']
    
    # Initialize a session using Boto3
    ec2 = boto3.client('ec2', region_name=region)
    
    # Stop the instances
    ec2.stop_instances(InstanceIds=instances)
    
    # Print the stopped instances
    print('Stopped your instances: ' + str(instances))

click Deploy.. Test .. create new event .. name: sample .. hello world .. save .. Test
Now, refresh ec2 page .. see ec2 stopping .. see at test, no error,,,, means lambda working well 

Start again .. lambda function .. file .. lambda.py file 
import boto3
def lambda_handler(event, context):
    region = 'us-east-1'
    instances = ['i-058c934ca37be80b2', 'i-0c6a8b96ce9383b91']    
    # Initialize a session using Boto3
    ec2 = boto3.client('ec2', region_name=region)    
    # Stop the instances
    ec2.start_instances(InstanceIds=instances)    
    # Print the stopped instances
    print('Started your instances: ' + str(instances))
Deploy > test >

# Thank you
