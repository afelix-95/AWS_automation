# AWS_automation
 Python scripts for automating important tasks in AWS cloud platforms.
## Instructions
Each folder has the script that can be used in AWS Lambda and the JSON file with the required IAM roles for each script.
### createEC2instances
This script will create the number of instances specified in `MaxCount` or the largest possible number of instances available above `MinCount`.
Before running the script, you need to create the following Environment Variables within Lambda:
* AMI = The ID of the virtual machine image that you want to use.
* INSTANCE_TYPE = The instance type that you want to use. It will determine the hardware of the host computer used for your instance.
* KEY_NAME = The name of the key pair needed to log into the instance.
* SUBNET_ID = The ID of the subnet to launch the instance into. If not specified, a default subnet from your default VPC will be chosen for you. 
That way, if you want to change these parameters afterwards, they can be changed directly without altering the code.
### resize_stored_imgs_S3
This script will copy any image uploaded to a S3 bucket and resize it to any dimensions specified in the code.
A trigger event needs to be setup in lambda where you define when the script will run. For this particular application, it will be whenever a new image is uploaded to the origin bucket.
Lastly, this script uses the PIL library, but it is not a standard library available in Python Lambda environment. Therefore, you need to follow these steps to make it work: 
* Install the precompiled Python package's .whl file that you can download at [Pillow's webpage](https://pypi.org/project/pillow/) as a dependency in your Lambda function's local directory.
* Create a Lambda deployment package .zip file archive that includes all of the installed libraries and source code.
* Use the .zip file archive to create a new Python Lambda function in the cloud environment.
### backupEC2instances
This script will take snapshots of all EC2 instances that are tagged for backup.
A rule can be created in CloudWatch to run this function at any scheduled time.
One thing to notice is that for this script the default Timeout of 3 sec won't be enough to run the Lambda function, so it has to be increased. By how much you have to increase it will depend on how many snapshots you will take.
### turn_on_off_EC2_instances
This directory has two separate functions, one for stopping all the currently running instances and another one for starting to run all the currently stopped instances. Both can be scheduled to run at different times with CloudWatch as well.
Once again, the default Timeout of 3 sec won't be enough to run either function, so it should be increased.
### backupDynamoDB
This script automates the backup of all the tables you have in DynamoDB. Again, this function can run on a schedule set as a rule in CloudWatch, you only need to specify the table name as an input when creating the rule.
