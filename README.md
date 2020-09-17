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
![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/EnableCloudTrail_Operations.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/EnableCloudTrail_Stack_Instances.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/EnableCloudTrail_Create_Complete_N.Virginia.png?raw=true)

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/EnableCloudTrail_Create_Complete_Oregon.png?raw=true)

Once the EnableCloudTrail StackSet is complete it is time to create the CreateVPC StackSet. First download this file to ones workstation https://github.com/aws-samples/aws-cloudformation-workshops/raw/master/workshop_1/CreateVpc.yaml. Make sure the file ends with the extension yaml. Once the file is downloaded, navigate to the administrative region in AWS CloudFormation and click on create stack. Note, do not click on create StackSet. On the specify template page, select template is ready and click on upload a template file. Choose the yaml file that was just downloaded and click next.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Specify_Tempalte.png?raw=true)

On the specify stack details page, name the stack CreateVPC and choose a subnet Availability Zone (AZ). When it’s ready click next.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Specify_Stack_Details.png?raw=true)

On the configure stack options page, leave the defaults and click Next. On the review page, click create stack. Once the stack is completed and one has seen that it will work, click on the CreateVpc stack and delete it. 

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Create_Complete_1.png?raw=true)

Next, open the CreateVpc.yaml file in a text editor and delete the following lines:
	Parameters:
	  AzName:
	    Type: AWS::EC2::AvailabilityZone::Name
	    Description: Subnet Availability Zone

Then replace the first line below with the three lines below it.
•	AvailabilityZone: !Ref AzName
•	AvailabilityZone: !Select
•	  - 0
•	  - !GetAZs ""

This allows the template to automatically select an AZ using the Select and GetAZs functions in CloudFormation. 

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC.yaml_Edit_1.png?raw=true)

Next, replace the first set of three groups of lines under the SubnetId, VpcId, and AzName with the second set of three groups of lines below them. 
•	SubnetId:
•	    Description: Subnet Id
•	    Value: !Ref PublicSubnet
•	
•	  VpcId:
•	    Description: Vpc Id
•	    Value: !GetAtt PublicSubnet.VpcId
•	
•	  AzName:
•	    Description: Subnet Availability Zone
•	    Value: !GetAtt PublicSubnet.AvailabilityZone

•	SubnetId:
•	    Description: Subnet Id
•	    Value: !Ref PublicSubnet
•	    Export:
•	      Name: SubnetId
•	
•	  VpcId:
•	    Description: Vpc Id
•	    Value: !GetAtt PublicSubnet.VpcId
•	    Export:
•	      Name: VpcId
•	
•	  AzName:
•	    Description: Subnet Availability Zone
•	    Value: !GetAtt PublicSubnet.AvailabilityZone
•	    Export:
•	      Name: AzName

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC.yaml_Edit_2.png?raw=true)

This is done to allow a stack to be used by other stacks. After the edits are complete, save the file. Then navigate back to AWS CloudFormation and create a stack using the new file. Take note that it doesn’t ask for an AZ on the specify stack details page this time because of the changes made to the file. 

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_No_Parameters.png?raw=true)

Continue through the steps and create the Stack. After you confirm it works, delete this stack as well.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Create_Complete_2.png?raw=true)

Now it’s time to establish peering between the two Virtual Private Clouds (VPC)s by making sure each VPC has a unique Classless Interdomain Routing (CIDR) block in the 10.0.0.0/8 space. To do this one must create a way to use different CIDR blocks for each region in which a stack is deployed in an automatic manner. This can be done in CloudFormation using mappings. To get started, save this file to ones workstation https://github.com/aws-samples/aws-cloudformation-workshops/raw/master/workshop_1/Mappings.yaml. Open the CreateVpc.yaml and Mappings.ymal on your workstation with an editor and paste the Mappings.yaml contents into the CreateVpc.yaml above the resources line. This is done to create a map named RegionMap that allows CloudFormation to use VpcCidr and SubnetCidr for the CIDR of the VPC and public subnet of each region.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Peering_Edit_1.png?raw=true)

To tell CloudFormation to use the values in the RegionMap replace the first set of three groups of lines under the Vpc, PublicSubnet, and Properties with the second set of three groups of lines below them. 
•	Vpc:
•	    Type: AWS::EC2::VPC
•	    Properties:
•	      CidrBlock: 10.200.0.0/16
•	      EnableDnsHostnames: 'true'
•	
•	  PublicSubnet:
•	    Type: AWS::EC2::Subnet
•	    Properties:
•	      AvailabilityZone: !Ref AzName
•	      CidrBlock: 10.200.11.0/24
•	      MapPublicIpOnLaunch: 'true'
•	      VpcId: !Ref Vpc

•	Vpc:
•	    Type: AWS::EC2::VPC
•	    Properties:
•	      CidrBlock: !FindInMap [ RegionMap, !Ref "AWS::Region", VpcCidr ]
•	      EnableDnsHostnames: 'true'
•	
•	  PublicSubnet:
•	    Type: AWS::EC2::Subnet
•	    Properties:
•	      AvailabilityZone: !Select
•	        - 0
•	        - !GetAZs ""
•	      CidrBlock: !FindInMap [ RegionMap, !Ref "AWS::Region", SubnetCidr ]
•	      MapPublicIpOnLaunch: 'true'
•	      VpcId: !Ref Vpc

When it’s ready, save the file.

![alt text](https://github.com/doyle199/AWS-CloudFormation-Multi-region-Deployment/blob/master/CreateVPC_Peering_Edit_2.png?raw=true)

When it’s ready, save the file. Now navigate back to AWS CloudFormation to deploy the new CreateVpc.yaml file as a stack like before. Go through all the steps to deploy and wait for the Stack to complete. In the newly deployed stack, click on the outputs tab. It shows that it created a VpcCidr and chose an AZ automatically because of the edits made in the file. After one confirms this information, delete the stack.

