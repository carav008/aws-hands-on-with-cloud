# Web Hosting Basics

Deploy a EC2 VM and host a simple static "Fortune-of-the-Day Coming Soon" web page.
- Services -> Compute -> EC2 -> Instances -> Launch Instances 

## Create Instance 
- Select AMI (amazon linux AMI (HVM) ami-0aeeebd8d2ab47354, t2.micro, TAKE defaults (for now) 
OR 
 	- Configure Instance Details 
		- Auto-assign Public IP: Enable
	- Add Storage (Take Default) 
	- Add Tags
		- Key: Type, Value: WebHostingBasics
 	- Configure Security Group
		- Create new security group 
			- Add SSH rule
			- Add HTTP rule 
	- Review and Launch 

	=> Select an existing key pair or create a new key pair 
		-> Create a new key pair  



### Install apache 

- Login into server with SSH key and run the following:  
NOTE: This can also be done using the user data but to learn all parts of process logging in is how this will be done here. 

```
$ sudo su -

$ yum install httpd -y 

$ systemctl enable httpd && systemctl start httpd 

$ echo "<h1> Welcome to Simple Web Hosting </h1>" >> /var/www/html/index.html 
```

Take a snapshot of your VM, delete the VM, and deploy a new one from the snapshot. Basically disk backup + disk restore.

## Take a snapshot of VM
https://docs.aws.amazon.com/prescriptive-guidance/latest/backup-recovery/ec2-backup.html
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-creating-snapshot.html
https://docs.aws.amazon.com/prescriptive-guidance/latest/backup-recovery/restore.html

### RESTORE FROM EBS VOLUME 
Navigate to Compute -> Elastic Block Store -> Snapshots 
- Create Snapshot 
	- Select resource type: instance 
	- instance ID: Select INSTANCE ID OF INSTANCE (will pop up)
  	- Descrtipion: Testing snapshots

- Create Volume 
	- On snapshot, under Actions select 'Create Volume'.
		- Select Size, Availability Zone, and Add Tags

- Create new Instance  
	- Select same Security Group as previous instance, 
	- Stop the Instance
	- Navigate to Compute -> Elastic Block Store -> Volumes	
		- Select Volume (currently) attached to instance, Select it and under Actions select 'Detach Volume' 
	- Now on Backup volume, select it and under Actions select 'Attach Volume'.o 
		- In this case we are assuming this is the root volume so select the Instance and Device Name to be /dev/xvda
	- Start instance again

- Validate
	- Navigate to new public http://IPv4_Address and verify that the web page is visible. 

Checkpoint: You can view a simple HTML page served from your EC2 instance.


# Hosting a Static Web Site 
https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html

### Create S3 Bucket 
- Create bucket 
	- Enter name, and take defaults. NOTE: name must be unique  

- Upload index.html and error.html 

- Make all objects Public 

- Configure Web Hosting on Bucket 

Huzzah after configuring you should be able to navigate to:  
- http://UNIQUE_BUCKET_NAME.s3-website-AWS_AZ.amazonaws.com/index.html
- http://UNIQUE_BUCKET_NAME.s3-website-AWS_AZ.amazonaws.com/error.html

