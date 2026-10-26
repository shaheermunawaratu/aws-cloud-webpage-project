# aws-cloud-webpage-project
Created seperate folders for each task.
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



