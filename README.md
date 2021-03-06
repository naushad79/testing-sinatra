# rea-sinatra
REA Sinatra Challenge

# Docker Image Build

## Prerequisite

Run docker in locally or any server that can be used to build and push the image to the AWS
ECR repo

# Create ECR Repo

Login to AWS account and create a ECR Repo
example : sample-app

# Build the image and push to repo

Goto the **docker** directory provided and run the following commands to build and push the image to the AWS ECR repo

**Note:** Make sure to configure AWS CLI and have access to the aws account and region is set to **ap-southeast-2**

`aws ecr get-login-password --region ap-southeast-2 | docker login --username AWS --password-stdin <replace_aws_account_id>.dkr.ecr.ap-southeast-2.amazonaws.com` <br/>
`# docker build -t sample-app .` <br/>
`# docker tag sample-app:latest<replace_aws_account_id>.dkr.ecr.ap-southeast-2.amazonaws.com/sample-app:latest` <br/>
`# docker push <replace_aws_account_id>.dkr.ecr.ap-southeast-2.amazonaws.com/sample-app:latest` <br/>

# Create AWS Infrastructure and Deploying the App
## Creating a task execution role

You could craete this role manually and set the **CreateRole** prameter value to **FALSE** or just skip this setep and run it with setting the parameter value to **TRUE**

1. Goto IAM from AWS Console
2. In the navigation pane, choose Roles, Create role.
3. In the Trusted entity type section, choose AWS service, Elastic Container Service.
4. For Use case, choose Elastic Container Service Task, then choose Next.
5. In the Attach permissions policy section, do the following:
   a. Search for AmazonECSTaskExecutionRolePolicy, then select the policy.
   b. Under Set permissions boundary - optional, choose Create role without a permissions boundary.
   c. Choose Next.
6. Under Role details, do the following:
   a. For Role name, type ecsTaskExecutionRole.
   b. For Add tags (optional), specify any custom tags to associate with the policy .
7. Choose Create role.

# Deploying the AWs Infrastructure and the App

Goto cf directory
Run the following command to deploy it
**Note:** Make sure to configure AWS CLI and have access to the aws account and region is set to *ap-southeast-2*

If you set decide to creat the task role via CF set the **CreateRole** to **TRUE**, if you have done it manually set it to **FALSE**
`# aws cloudformation create-stack --stack-name sample-app --template-body file://sample-app.yaml --parameters ParameterKey=ImageName,ParameterValue=<replace_aws_account_id>.dkr.ecr.ap-southeast-2.amazonaws.com/sample-app:latest ParameterKey=CreateRole,ParameterValue='TRUE' --capabilities CAPABILITY_NAMED_IAM`

# Accessing the App

- From AWS console, goto cloudformation
- Select the sample-app stack
- Goto output tab
- Click on the url to access the app
