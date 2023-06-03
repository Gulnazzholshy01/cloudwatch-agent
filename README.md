# Installing CloudWatchAgent on EC2 

## STEPS

## Step 1. Create an IAM Role for EC2 Instance that enables agent to collect metrics from the server, to do so:

* 1. You can create an IAM Role in the Console and attach to EC2 Instance after launching CF template
* 2. Or you can create an IAM Role in the Console and attach it through CF template
* 3. Or you can create and attach IAM Role in the CF template itself
##### I'm going with the second option

##### Note! You can not directly attach an IAM Role to an EC2 Instance, you might think why? 

> As AWS says, An application running on an EC2 instance is abstracted from AWS by the virtualized operating
system. Because of this extra separation, an additional step is needed to assign an AWS role and its associated permissions to an EC2 instance. This extra step is the creation of an instance profile that you can attach to an EC2 instance.

##### Instance profile is nothing but the container for an IAM role:
* You can use an instance profile to pass an IAM role to an EC2 instance
* An instance profile can only contain one IAM role. However, please note that a role can belong to multiple instance profiles

> Note! When you attach IAM Role in the Console, it creates both an EC2 instance profile as well as an IAM role with the same name. But when you are using the AWS CLI, SDKs, or CloudFormation, you will need to define both of them explicitly. 

#### How it works?
> Create a Role/Take an existing Role –> Put it into an Instance Profile –> Attach instance Profile to EC2 instance.

## Step 2. SSH to your instance and install cloudwatch agent:

``` sudo yum install amazon-cloudwatch-agent -y ```

##### Now you have cloudwatch agent installed, you can check its status:

``` sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status ```

## Step 3. Run the CloudWatch agent configuration wizard

``` sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard ```

##### Here you have to answer several questions to customize cloudwatch agent config file:

*  I chose Basic metrics as it collects metrics of memory and disk_usage
* log file path: /var/log/messages
* give a name for your log group

## Step 4. Now you have your file configured, then start the CloudWatchAgent
``` sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:configuration-file-path```

##### Note, CLW config file location: /opt/aws/amazon-cloudwatch-agent/bin
##### You can check the status of cloudwatch agent again, if you want
##### If everything is CORRECT, you see the "configuration validation  succeeded" message

## Step 5. Go to CloudWatch dashboard and check the metrics 

