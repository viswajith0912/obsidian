Creating tiggers to start and stop Ec2 instances using lambda functions in AWS

Overview of the Process

1. Create an IAM Policy: This policy will grant the necessary permissions for our Lambda functions to start and stop EC2 instances.
    
2. Create an IAM Role: create a role that our Lambda functions can assume to get the permissions defined in the IAM policy.
    
3. Create a Lambda Function to Start EC2 Instances: This function will contain the code to start your targeted EC2 instances.
    
4. Create a Lambda Function to Stop EC2 Instances: This function will contain the code to stop your targeted EC2 instances.
    
5. Create Amazon EventBridge Triggers: These triggers will schedule the execution of our Lambda functions at specific times.

step 1: Create an IAM Policy for EC2

An IAM (Identity and Access Management) policy is a document that formally states permissions. a policy that allows starting and stopping EC2 instances.

1. Navigate to the IAM service in the AWS Management Console.
    
2. In the left navigation pane, choose Policies, then click Create policy.
    
3. Select the JSON tab and paste the following policy document. This policy grants permissions to start, stop, and describe EC2 instances, as well as to log events in CloudWatch.

![[Pasted image 20250822102121.png]]

4. Click Next: Tags, then Next: Review.
    
5. For the Name, enter a descriptive name like EC2StartStopPolicy.
    
6. Provide an optional Description.
    
7. Click Create policy.

Step 2: Create an IAM Role

An IAM role is an identity that you can create in your account that has specific permissions. In this case, our Lambda functions will assume this role.

1. In the IAM console, select Roles from the left navigation pane and click Create role.
    
2. For the trusted entity type, choose AWS service.
    
3. For the use case, select Lambda, then click Next.
    
4. In the Add permissions section, search for and select the EC2StartStopPolicy you created in the previous step.
    
5. Click Next.
    
6. For the Role name, enter something descriptive, such as EC2StartStopLambdaRole.
    
7. Provide an optional Description.
    
8. Click Create role.

Step 3: Create the Lambda Function to Start EC2 Instances

This function will contain the Python code to start your instances.

1. Navigate to the Lambda service in the AWS Management Console.
    
2. Click Create function.
    
3. Select Author from scratch.
    
4. For the Function name, enter StartEC2Instances.
    
5. For the Runtime, select Python 3.9 (or a later version).
    
6. Under Permissions, expand Change default execution role.
    
7. Choose Use an existing role and select the EC2StartStopLambdaRole you created.
    
8. Click Create function.
    
9. In the Code source section, replace the existing code in lambda_function.py with the following:

![[Pasted image 20250822102428.png]]

10.  Click Deploy to save your changes.

Step 4: Create the Lambda Function to Stop EC2 Instances

This process is very similar to creating the start function.

1. In the Lambda console, click Create function.
    
2. Select Author from scratch.
    
3. For the Function name, enter StopEC2Instances.
    
4. For the Runtime, select Python 3.9 (or a later version).
    
5. Under Permissions, choose Use an existing role and select the EC2StartStopLambdaRole.
    
6. Click Create function.
    
7. In the Code source section, replace the existing code with the following:

![[Pasted image 20250822102642.png]]

8. Click Deploy.

Step 5: Create Amazon EventBridge Triggers

schedule Lambda functions to run at specific times using Amazon EventBridge.

Trigger for Starting Instances

1. Navigate to the Amazon EventBridge service in the AWS Management Console.
    
2. In the left navigation pane, select Rules, then click Create rule.
    
3. For the Name, enter a descriptive name like StartEC2InstancesSchedule.
    
4. In the Define pattern section, select Schedule.
    
5. Choose Cron expression and enter a cron job to define when your instances should start. For example, to start instances at 8 AM UTC Monday through Friday, you would enter: cron(0 8 ? * MON-FRI *).
    
6. Click Next.
    
7. In the Select target(s) section, for the Target type, choose AWS service.
    
8. For the Select a target dropdown, choose Lambda function.
    
9. For the Function, select the StartEC2Instances function.
    
10. Click Next, then Next again, and finally Create rule.
    

Trigger for Stopping Instances

1. Follow the same steps as above to create another rule.
    
2. For the Name, enter a descriptive name like StopEC2InstancesSchedule.
    
3. For the Cron expression, enter a schedule for stopping the instances. For example, to stop instances at 6 PM UTC Monday through Friday, you would enter: cron(0 18 ? * MON-FRI *).
    
4. For the Target, select the StopEC2Instances Lambda function.
    
5. Complete the rule creation process.