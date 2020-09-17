# AWS-CloudFormation-Multi-region-Deployment

__AWS CloudFormation Dual-Region StackSets Deployment__
	
To demonstrate the power of AWS CloudFormation, the following will detail the process of deploying a dual-region StackSet. Before one can begin creating AWS CloudFormation StackSets, one must have access to an AWS account with administrator access. It is best practice to make an IAM user with the correct access instead of using the root account. To do this navigate to the IAM console and create a new user. Give the user a name, the correct access, in this case administrative access, and a password. For this demonstration the user name will be CCA670Admin. Once this is complete log in as the new user.
	
To get started, AWS regions must be chosen to deploy the stacks into. An administrative region and two target regions must be chosen. In this case the administrative region will be us-east-1 (N. Virginia) and the two target regions will be us-east-1 (N. Virginia) and us-west-2 (Oregon). 
After the two regions are chosen one must locate the IAM account ID number. It can be found easily by looking at the top of the AWS Console webpage near the username. There is an option to select a dropdown there which shows the ID number.

In order to deploy effectively across regions, one must first set up two IAM roles to be used for AWS CloudFormation StackSets. These two IAM role are AWSAWSCloudFormationStackSetAdministrationRole and AWSCloudFormationStackSetExecutionRole. To create these roles, one must deploy two AWS CloudFormation templates. Navigate to the IAM console and click on roles in the left menu. Once inside, search for this role: AWSCloudFormationStackSetAdministrationRole.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/AWS_CloudFormation_StackSet_Administration_Role.png?raw=true=)
