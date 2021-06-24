Auto Scaling

Create an AMI from that VM and put it in an autoscaling group so one VM always exists.

## Create the AMI 
Navigate to Compute => EC2 => Instances
=> Select the Instance => Under 'Actions' => Image and templates => Create Image

Now you can enter the following: 
- Image Name, Image Description and add any necessary Tags

Navigate to Compute => EC2 => Images, You should be able see the image being created.  

NOTE: you will need to copy, note down the AMI id for configuration of next step. 


## Create Autoscaling group
https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html

### create Launch Template 

In order to create an Auto Scaling group we must first create a Launch Template. A Launch Tmeplate is just as it sounds, it acts as the configuration blueprint for the EC2 instances that will be launched within the Auto Scaling group.  

Navigate to Compute => EC2 => Instances => Launch Templates => Create Launch Template 
- Launch Template Name 
- Template version description 
- Check the box 'Provide guidance to help me set up a template that I can use with EC2 Auto Scaling' 
- Now under the 'Amazon machine image (AMI) - required' 
	- Select the AMI that was recently created 
- Select the "Instance Type" (optional) 
- Select a Key Pair (optional) 
- Under the 'Network settings' select the security group that was initially created with the inital instance. 
- Add Tags 
=> Hit "Create Launch Template"  

### create Auto Scaling Group 
Navigate to Compute => EC2 => Auto Scaling => Auto Scaling Groups  
=> Create Auto Scaling Group 
- Enter 'Auto Scaling group name': 'WebHostingASG'
- Select the 'Launch Template' that was created in the steps above  
- For the 'Instance purchase options' select 'Adhere to Launch Template' 
- In the 'Network' section 
	- Select a VPC 
	- Select 2 subnets  

- Configure Advanced options 
	- Load balancing: for now you can select 'No load Balancer', the next step will be to configure the load balancer 

- Configure group size and scaling policies
	-  Under the 'Group Size - optional', select 2 for each (desired, minimum, and maximum' 
	- Take defaults for the rest of sections

- Add Tags

### Testing the auto scaling group 
Now that the auto scaling is all configured, the instances should start to be spun up or are already running. To test out the auto scaling group Terminate one instance, there should be another auto scaling instance being created 
NOTE: if you navigate to the Auto Scaling group you should see it have a 'Status' of 'Updating Capacity' after termination of one instance 


Put a Elastic Load Balancer infront of that VM and load balance between two Availability Zones (one EC2 in each AZ).


Checkpoint: You can view a simple HTML page served from both of your EC2 instances. You can turn one off and your website is still accessible.
