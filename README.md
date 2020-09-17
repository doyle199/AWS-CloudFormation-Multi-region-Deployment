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

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/AWS%20CloudFormation_StackSet_Administration_Role_Review_Page.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/AWS_CloudFormation_StackSet_Administration_Role_Stack_Completion.png?raw=true)

Once the stack completes, navigate to the IAM console and click on roles in the left menu once more. Search for AWSCloudFormationStackSetExecutionRole.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/AWS_CloudFormation_StackSet_Execution_Role.png?raw=true)

If the role doesn’t exist, navigate to the CloudFormation console. Make sure that one is in the administrative region and click create stack. For the specify template section, select template is ready and amazon S3 URL like before. Then enter the following URL into the Amazon S3 URL box: https://s3.amazonaws.com/cloudformation-stackset-sample-templates-us-east-1/AWSCloudFormationStackSetExecutionRole.yml. After it’s ready click next.
For the specify stack details page enter StacksetExecutionRole as the stack name. This time, one needs to enter the IAM account ID number in the parameters box and then click next.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/StackSet_Execution_Role_Specify_Stack_Details.png?raw=true)

Leave default settings on the configure stack options page and click next. On the review page check the acknowledgement box and click create stack.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/StackSet_Execution_Role_Stack_Completion.png?raw=true)

After the stack is completed one must create the EnableCloudTrail StackSet. This will deploy into the two target regions. In the CloudFormation administrator region, select StackSet in the left menu and click create StackSet.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Enable_CloudTrail_StackSet.png?raw=true)

Once inside choose use a sample template and click on the dropdown for choose a sample template. In the dropdown choose enable AWS CloudTrail and click next.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Enable_CloudTrail_Choose_a_Template_1.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Enable_CloudTrial_Choose_a_Template_2.png?raw=true)

On the specify StackSet details page, name the stack EnableCloudTrail, leave the rest of the defaults, and click next. Take note that it only asks for parameters once. This will be explained later.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Enable_CloudTrail_Specify_StackSet_Details.png?raw=true)

On the configure StackSet options page, click on self-service permissions and choose the AWSCloudFormationStackSetAdministrationRole. Make sure the AWSCloudFormatioStack-SetExecutionRole is in the box for IAM execution and click next.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Enable_CloudTrial_Configure_StackSet_Options.png?raw=true)

On the set deployment options page, choose deploy stacks in accounts and enter your IAM account ID number in the box. Scroll down to the specify regions section and add the two regions. For this demonstration the regions are N Virginia and Oregon. Leave the rest at defaults and click next.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Enable_CloudTrail_Set_Deployment_Options_1.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Enable_CloudTrail_Set_Deployment_Options_2.png?raw=true)

On the review page click submit. The StackSet will fail to deploy because of mistakes in the TrailBucketPolicy of the YAML file.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/TrailBucketPolicy_StackSet_Create_Failed.png?raw=true)

To fix this, one must edit the StackSet template file. Click into the failed EnableCloud-Trail StackSet and click the template tab. Click the copy to clipboard button on the right. Then delete the first StackSet by clicking the actions dropdown and deleting the Stacks in the StackSet. It will ask for your IAM account ID number. After the Stacks are deleted click the dropdown again to delete the StackSet.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Failed_EnableCloudTrail_Template_Location.png?raw=true)

Take the template information one copied and paste it into a text editor. Scroll down to the TrailBucketPolicy section and replace the asterisk * with the letters aws. The spacing matters in YAML so make sure to keep it in the right format. After the adjustments have been made, save it as a YAML file. 

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/TrailBucketPolicy_Template_Fix.png?raw=true)

Navigate to the AWS S3 console and create a new S3 bucket. Upload the YAML file that was just fixed to the bucket. Once the file is uploaded, copy the Object URL.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/AWS_S3_Object_URL.png?raw=true)

After the Object URL is copied, navigate back to CloudFormation StackSets. Click create StackSet. Select template is ready and Amazon S3 URL. Paste the Object URL into the box for Amazon S3 URL. When it’s ready click next and go through the steps as before.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Amazon_S3_URL.png?raw=true)

This time the StackSet deployment will succeed. Check the EnableCloudTrail operations tab for a green succeeded statement. Then click the stack instances tab to see that the two instances are in a green current status. To make sure the StackSet deployed to both target regions navigate to both regions and check the Stacks for a green create complete status. 
