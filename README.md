# AWS-CloudFormation-Multi-region-Deployment

__AWS CloudFormation Dual-Region StackSets Deployment__
	
To demonstrate the power of AWS CloudFormation, the following will detail the process of deploying a dual-region StackSet. Before one can begin creating AWS CloudFormation StackSets, one must have access to an AWS account with administrator access. It is best practice to make an IAM user with the correct access instead of using the root account. To do this navigate to the IAM console and create a new user. Give the user a name, the correct access, in this case administrative access, and a password. For this demonstration the user name will be CCA670Admin. Once this is complete log in as the new user.
	
To get started, AWS regions must be chosen to deploy the stacks into. An administrative region and two target regions must be chosen. In this case the administrative region will be us-east-1 (N. Virginia) and the two target regions will be us-east-1 (N. Virginia) and us-west-2 (Oregon). 
After the two regions are chosen one must locate the IAM account ID number. It can be found easily by looking at the top of the AWS Console webpage near the username. There is an option to select a dropdown there which shows the ID number.

In order to deploy effectively across regions, one must first set up two IAM roles to be used for AWS CloudFormation StackSets. These two IAM role are AWSAWSCloudFormationStackSetAdministrationRole and AWSCloudFormationStackSetExecutionRole. To create these roles, one must deploy two AWS CloudFormation templates. If the role doesn’t exist navigate to the AWS CloudFormation console. Make sure you’re in one’s administrative region, which in this case is N. Virginia, and click create stack.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/AWS_CloudFormation_StackSet_Administration_%20Role_Create_Stack.png?raw=true)

For the specify template section, select template is ready and Amazon S3 URL. Paste the following URL into the Amazon S3 URL box: https://s3.amazonaws.com/cloudformation-stackset-sample-templates-us-east-1/AWSCloudFormationStackSetAdministrationRole.yml. After it’s ready click next.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/AWS_CloudFormation_StackSet_Administration_Role_Specify_Template.png?raw=true)

For the specify stack details section enter StacksetAdministrationRole for the stack name and click next.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/AWS_CloudFormation_StackSet_Administration_Role_Specify_Stack_Details.png?raw=true)

Leave the defaults on the configure stack options page and click Next. On the review page check the acknowledge box and click create stack.

