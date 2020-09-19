# AWS-CloudFormation-Multi-region-Deployment

__AWS CloudFormation Dual-Region StackSets Deployment__
	
To demonstrate the power of AWS CloudFormation, the following will detail the process of deploying a dual-region StackSet. Before one can begin creating AWS CloudFormation StackSets, one must have access to an AWS account with administrator access. It is best practice to make an IAM user with the correct access instead of using the root account. To do this navigate to the IAM console and create a new user. Give the user a name, the correct access, in this case administrative access, and a password. For this demonstration the user name will be CCA670Admin. Once this is complete, log in as the new user.
	
To get started, AWS regions must be chosen to deploy the stacks into. An administrative region and two target regions must be chosen. In this case the administrative region will be us-east-1 (N. Virginia) and the two target regions will be us-east-1 (N. Virginia) and us-west-2 (Oregon). 
After the two regions are chosen one must locate the IAM account ID number. It can be found easily by looking at the top of the AWS Console webpage near the username. There is an option to select a dropdown there, which shows the ID number.

In order to deploy effectively across regions, one must first set up two IAM roles to be used for AWS CloudFormation StackSets. These two IAM roles are AWSAWSCloudFormationStackSetAdministrationRole and AWSCloudFormationStackSetExecutionRole. To create these roles, one must deploy two AWS CloudFormation templates. If the role doesn’t exist, navigate to the AWS CloudFormation console. Make sure you’re in one’s administrative region, which in this case is N. Virginia, and click create stack.

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

If the role doesn’t exist, navigate to the CloudFormation console. Make sure that one is in the administrative region and click create stack. For the specify template section, select template is ready and amazon S3 URL like before. Then paste the following URL into the Amazon S3 URL box: https://s3.amazonaws.com/cloudformation-stackset-sample-templates-us-east-1/AWSCloudFormationStackSetExecutionRole.yml. After it’s ready click next.

For the specify stack details page enter StacksetExecutionRole as the stack name. This time, one needs to enter the IAM account ID number in the parameters box and then click next.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/StackSet_Execution_Role_Specify_Stack_Details_2.png?raw=true)

Leave default settings on the configure stack options page and click next. On the review page check the acknowledgement box and click create stack.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/StackSet_Execution_Role_Stack_Completion.png?raw=true)

After the stack is completed one must create the EnableCloudTrail StackSet. This will deploy into the two target regions. In the CloudFormation administrator region, select StackSet in the left menu and click create StackSet.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Enable_CloudTrail_StackSet.png?raw=true)

Once inside choose use a sample template and click on the dropdown for choose a sample template. In the dropdown choose enable AWS CloudTrail and click next.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Enable_CloudTrail_Choose_a_Template_1.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Enable_CloudTrial_Choose_a_Template_2.png?raw=true)

On the specify StackSet details page, name the stack EnableCloudTrail, leave the rest of the defaults, and click next. Take note that it only asks for parameters once. This will be explained later.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Enable_CloudTrail_Specify_StackSet_Details.png?raw=true)

On the configure StackSet options page, click on self-service permissions and choose the AWSCloudFormationStackSetAdministrationRole. Make sure the AWSCloudFormatioStackSetExecutionRole is in the box for IAM execution and click next.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Enable_CloudTrial_Configure_StackSet_Options.png?raw=true)

On the set deployment options page, choose deploy stacks in accounts and enter your IAM account ID number in the box. Scroll down to the specify regions section and add the two regions. For this demonstration the regions are N Virginia and Oregon. Leave the rest at defaults and click next.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Enable_Cloud_Trail_Set_Deployment_Options_1_2.png?raw=true)

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
![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/EnableCloudTrail_Operations.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/EnableCloudTrail_Stack_Instances.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/EnableCloudTrail_Create_Complete_N.Virginia.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/EnableCloudTrail_Create_Complete_Oregon.png?raw=true)

Once the EnableCloudTrail StackSet is complete it is time to create the CreateVPC StackSet. First download this file to ones workstation (right click and save as) https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVpc.yaml. Make sure the file ends with the extension yaml. Once the file is downloaded, navigate to the administrative region in AWS CloudFormation and click on create stack. Note, do not click on create StackSet. On the specify template page, select template is ready and click on upload a template file. Choose the yaml file that was just downloaded and click next.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Specify_Tempalte.png?raw=true)

On the specify stack details page, name the stack CreateVPC and choose a subnet Availability Zone (AZ). When it’s ready click next.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Specify_Stack_Details.png?raw=true)

On the configure stack options page, leave the defaults and click Next. On the review page, click create stack. Once the stack is completed and one has seen that it will work, click on the CreateVpc stack and delete it. 

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Create_Complete_1.png?raw=true)

Next, open the CreateVpc.yaml file in a text editor and delete the following lines:

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Parameters_1.png?raw=true)

Then replace the first line below with the three lines below it.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Availability_1.png?raw=true)

This allows the template to automatically select an AZ using the Select and GetAZs functions in CloudFormation. 

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC.yaml_Edit_1.png?raw=true)

Next, replace the first set of three groups of lines under the SubnetId, VpcId, and AzName with the second set of three groups of lines below them. 

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/SubnetID_1.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC.yaml_Edit_2.png?raw=true)

This is done to allow a stack to be used by other stacks. After the edits are complete, save the file. Then navigate back to AWS CloudFormation and create a stack using the new file. Take note that it doesn’t ask for an AZ on the specify stack details page this time because of the changes made to the file. 

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_No_Parameters.png?raw=true)

Continue through the steps and create the Stack. After you confirm it works, delete this stack as well.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Create_Complete_2.png?raw=true)

Now it’s time to establish peering between the two Virtual Private Clouds (VPC)s by making sure each VPC has a unique Classless Interdomain Routing (CIDR) block in the 10.0.0.0/8 space. To do this one must create a way to use different CIDR blocks for each region in which a stack is deployed in an automatic manner. This can be done in CloudFormation using mappings. To get started, save this file to ones workstation (right click and save as) https://github.com/aws-samples/aws-cloudformation-workshops/raw/master/workshop_1/Mappings.yaml. Open the CreateVpc.yaml and Mappings.ymal on your workstation with an editor and paste the Mappings.yaml contents into the CreateVpc.yaml above the resources line. This is done to create a map named RegionMap that allows CloudFormation to use VpcCidr and SubnetCidr for the CIDR of the VPC and public subnet of each region.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Peering_Edit_1.png?raw=true)

To tell CloudFormation to use the values in the RegionMap replace the first set of three groups of lines under the Vpc, PublicSubnet, and Properties with the second set of three groups of lines below them. 

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/VPC_1.png?raw=true)

When it’s ready, save the file.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Peering_Edit_2.png?raw=true)

When it’s ready, save the file. Now navigate back to AWS CloudFormation to deploy the new CreateVpc.yaml file as a stack like before. Go through all the steps to deploy and wait for the Stack to complete. In the newly deployed stack, click on the outputs tab. It shows that it created a VpcCidr and chose an AZ automatically because of the edits made in the file. After one confirms this information, delete the stack.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Stack_Outputs.png?raw=true)

Now it’s time to deploy the CreateVpc.yaml as a StackSet by selecting the template is ready and the uploading the template file options on the choose a template page. On the specify StackSet details page give the StackSet the name CreateVpc then click next. On the configure StackSet options page, click on self-service permissions and choose the AWSCloudFormation-StackSetAdministrationRole. Make sure the AWSCloudFormatio-StackSetExecutionRole is in the box for IAM execution and click next. On the set deployment options page, choose deploy stacks in accounts and enter your IAM account ID number in the box. Scroll down to the specify regions section and add the two regions. For this demonstration the regions are N Virginia and Oregon. Leave the rest at defaults at click next. On the last page review and then click submit. 

Check the CreateVpc operations tab for a green succeeded statement. Then click the stack instances tab to see that the two instances are in a green current status. To make sure the StackSet deployed to both target regions navigate to CloudFormation in both N. Virginia and Oregon and check the Stacks for a green create complete status. Each stack also shows that it created a VpcCidr and chose an AZ in the Outputs tab.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Operations.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Stack_Instances.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Create_Complete_N.Virginia.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Create_Complete_Oregon.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_StackSet_Outputs_N.Virginia.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_StackSet_Outputs_Oregon.png?raw=true)

Now that the CreateVpc StackSet is deployed and working, it’s time to create the LAMP StackSet. To get started, download this file to your workstation(right click and save as) https://github.com/aws-samples/aws-cloudformation-workshops/raw/master/workshop_1/Lamp.yaml. This LAMP file will have a parent stack created to build out the Lamp.yaml as a nested stack. The parent stack will import the AzName, VpcId, and SubnetId values form the CreateVpc StackSet. When you use nested stacks the stack template must be in an S3 bucket in the same region. Next, download this file to your workstation (right click and save as) 
https://github.com/aws-samples/aws-cloudformation-workshops/raw/master/workshop_1/LampParent.yaml. The file uses the ImportValue intrinsic function for AzName, VpcId, and SubnetId from the CreateVpc StackSet in the region.

Let’s set up an S3 bucket. Navigate to S3 and create a bucket. Upload the Lamp.yaml file to the bucket and copy the Object URL in the overview tab. Once one has copied the Object URL, open LampParent.yaml in a text editor. Replace the S3 Object URL in the resources section with the one that was copied. When ready, save the edited LampParent.yaml file.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/S3_LampParent.yaml_File_Edit.png?raw=true)

Deploy the edited LampPartent.yaml as a stack in the administrative region. On the specify stack details page, name the stack LampParent and create a password. Once it’s ready click next.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/LampParent.yaml_Specify_Stack_Details.png?raw=true)

Go through the rest of the setup and check the two acknowledgement boxes on the review page. Once everything is ready, click create stack.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/LampParent.yaml_Acknowledgements.png?raw=true)

After deployment completes, click the outputs tab and then click on the LampInstaceUrl to display the working test webpage.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/LampParent_LampInstanceURL.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/LampParent_Working_Test_Webpage.png?raw=true)

Once it is confirmed to be working, delete the LampParent Stack, this will also delete the nested stack. Next, it’s time to have AWS CloudFormation generate random string passwords with a minimum length of eight alphabetic characters. To get started, download the Random.yaml file to your workstation (right click and save as) https://github.com/aws-samples/aws-cloudformation-workshops/raw/master/workshop_1/Random.yaml. This will allow a Lambda function to create a password. 

Open the updated LampParent.yaml file in a text editor and remove the entire parameters section. Then insert all of the Random.yaml file contents into the LampParent.yaml file just below the Resources line.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/LampParent_Random_File_Edit.png?raw=true)

In the editor, locate the LampStack resource section and replace this first line with the second line.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/DBRootPassword.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/LampParent_File_Edit.png?raw=true)

Find the output section and add the following lines.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/DBPassword2.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/LampParent_File_Edit_2.png?raw=true)

Save the LampParent.yaml file and deploy it as a stack in AWS CloudFormation in the administrative region like before. When it’s complete, in the stack output tab you can see an automatically created password eight characters long next to DbRootPassword. 

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/LampParent_Stack_Create_Complete.png?raw=true)

Once one has confirmed it works, delete the stack. Now deploy the LampParent.yaml file as a StackSet. Wait for its completion. Check the LampParent StackSet operations tab for a green succeeded statement. Then click the stack instances tab to see that the two instances are in a green current status. To make sure the StackSet deployed to both target regions navigate to CloudFormation in both N. Virginia and Oregon and check the stacks for a green create complete status. Each stack also shows that it has a unique password eight characters long because of the file edit. One can also check the two regions to see the running instances and VPCs. 

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/LampParent_StackSet_Operations.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/LampParent_StackSet_Stack_Instances.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/LampParent_Stack_Outputs_N.Virginia.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/LampParent_Stack_Outputs_Oregon.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Stack_Instance_N.Virginia.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Stack_Instance_Oregon.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Stack_VPC_N.Virginia.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/Stack_VPC_Oregon.png?raw=true)

To clean up the resources used, navigate to the StackSets in the administrative region. In a StackSet click on the actions dropdown and choose delete Stacks form the StackSet. After that completes the StackSet can be deleted. Delete any remaining stacks in both regions and any relevant S3 buckets.

After fallowing these steps, one has deployed a dual-region environment using AWS CloudFormation StackSets. Both regions had a LAMP stack created which included a VPC and Amazon Linux 2 in each region. 



