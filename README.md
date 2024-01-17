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
This script will copy any image uploaded to a S3 bucket and resize it to any dimensions specified in 
A trigger event needs to be setup in lambda where you define when the script will run. For this particular application, it will be whenever a new image is uploaded to the origin bucket.
Lastly, this script uses the PIL library, but it is not a standard library available in Python Lambda environment. Therefore, you need to follow these steps to make it work: 
* Install the precompiled Python package's .whl file that you can download at [Pillow's webpage](https://pypi.org/project/pillow/) as a dependency in your Lambda function's local directory.
* Create a Lambda deployment package .zip file archive that includes all of the installed libraries and source code.
* Use the .zip file archive to create a new Python Lambda function in the cloud environment.
### backupEC2instances
--To be continued--
