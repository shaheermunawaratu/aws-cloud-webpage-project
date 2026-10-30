# aws-cloud-webpage-project
Created seperate folders for each task.

Task 1:

Used chatgpt to get a plan for the workflow using this command;
I'm doing a CloudFormation lab for uni, I have to build two things in AWS using YAML templates, first a static website. the HTML/CSS lives in an S3 bucket and gets served publicly, I'm thinking CloudFront with the bucket kept private. the page header has to say my L number and my name, that's part of the marking. 2nd a separate 3 tier setup with a web tier, app tier and database tier. so a VPC, public/private/isolated subnets across two AZs, a load balancer, auto scaling groups and RDS. I want to create the folders project-1-static-website/website and project-2-three-tier, with a .gitkeep in each so git keeps them, and add a .gitignore that suits AWS work (keep .env, .pem files, .aws/ and .DS_Store out). just create or edit files

The first command it gave to create the folder failed because it gave for bash syntax, again tried for cmd and it worked.

2nd command to chatgpt;
Now i want to create the cloudformation script in yaml to create the website, the webpage should show my Name and my L number. Also store the contents of the webpage in S3 bucket. I also want to use a public web service to host my website

used the youtube tutorial to connect aws cli with cmd:
http://youtube.com/watch?v=jCHOsMPbcV0

Succeccfully created the website and deployed it in S3 bucket;
https://d31q06zw18x3h0.cloudfront.net 

Checked that the bucket is private and secure; 
{                                                                     
    "PublicAccessBlockConfiguration": {
        "BlockPublicAcls": true,
        "IgnorePublicAcls": true,
        "BlockPublicPolicy": true,
        "RestrictPublicBuckets": true
    }
}



Task 2:
Chatgpt command:
"I am starting the second part of the lab and I already have the folder named as project 2-three-tier so now I want you to create the template.yaml file with just the network stuff my region is eu-north-I want a VPC uh an Internet gateway and two availability zones use get AZs and select, don't hardcore the zone names. *In each zone* I need a public subnet for the webtier, a private one for the app tier and isolated one for the database. public goes to the Internet gateway private goes out through an isolated gets no Internet at all. NAT cost money so add a parameter where I can pick one Nat or one per zone. put the cidrs and environment name in parameter tag everything properly and output the VPC ID and subnet IDs add a comment above each resourcing why it's there then run cfn-lint and fix anything it moans about just edit the files please no get or no commands I'll do that myself"
First I designed and deployed a web-tier network on AWS using CloudFormation and AWS CLI in the eu-north-1 region. The infrastructure includes a VPC with six subnets across two Availability Zones, separating the web, application and database tiers. I configured an Internet Gateway, NAT Gateway and route tables, with a parameter to choose between one NAT Gateway or one per Availability Zone. I also removed the stack after testing to avoid unnecessary AWS charges.

Chatgpt command for App tier: 
"For the app tier, same file again, keep everything that's already there. I want an auto scaling group in the private subnets running a tiny demo app, like a little python or node server that returns some json, set up in the user data. only let the app port in from the web tier's security group, nothing from the internet. give it a least privilege IAM role with SSM access, IMDSv2 required again, and a CPU scaling policy. the web tier needs to be able to reach the app tier, so if that needs an internal load balancer then add one and explain why in a comment. comment everything and run cfn-lint at the end. no git or aws commands from you"

After completing the network setup, I updated the CloudFormation template to deploy the complete three-tier infrastructure. I configured an internet-facing Application Load Balancer with Nginx web servers running on EC2 instances managed by an Auto Scaling Group. I then deployed an internal Application Load Balancer and a Python demo application in the private application tier, also using an Auto Scaling Group. I tested the public endpoint successfully, which returned HTTP 200 and confirmed that requests were reaching the Python application through the web tier. Finally, I deployed an Amazon RDS MySQL database in isolated subnets with public access disabled. The database is running successfully, but connecting it to the Python application and testing database operations such as INSERT and SELECT are still pending.

there were some errors because of IAM permissions but it's already corrected.

I asked ChatGPT to help me integrate the existing Amazon RDS MySQL database into my AWS three-tier architecture using AWS CloudFormation. The work involved creating a Secrets Manager secret for the existing database credentials, configuring a dedicated IAM role and instance profile with the required permissions, updating the application EC2 launch template, and modifying the Python application to retrieve credentials securely and connect to RDS using PyMySQL. We also configured an Auto Scaling instance refresh, deployed the changes through a CloudFormation change set, and tested the `/db-health` endpoint to confirm a successful database connection. During the process, we faced YAML indentation and validation issues, an IAM permission error when CloudFormation attempted to attach an AWS-managed policy, and a temporary AWS endpoint connectivity issue. We resolved these problems by correcting the YAML, replacing the managed policy attachment with inline IAM permissions, and monitoring the CloudFormation update. We also verified that the existing RDS database was not being replaced or modified. The final database health check returned HTTP 200 with `"database": "connected"`.