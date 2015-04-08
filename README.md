# codedeploy
aws code deploy sample repo

#Instruction
1.) Login to AWS console.

2.) Create Two Roles in IAM.

2.1.) Create Custom Policy for codedeploy with the below template eg : role-codedeploy.

>			{
>			  "Version": "2012-10-17",
>			  "Statement": [
>				{
>				  "Action": [
>					"autoscaling:PutLifecycleHook",
>					"autoscaling:DeleteLifecycleHook",
>					"autoscaling:RecordLifecycleActionHeartbeat",
>					"autoscaling:CompleteLifecycleAction",
>					"autoscaling:DescribeAutoscalingGroups",
>					"autoscaling:PutInstanceInStandby",
>					"autoscaling:PutInstanceInService",
>					"ec2:Describe*"
>				  ],
>				  "Effect": "Allow",
>				  "Resource": "*"
>				}
>			  ]
>			})

2.2.) Edit the "Trust Relationships" of the above role (role-codedeploy) and replace it with below teamplate.

>			{
>			  "Version": "2012-10-17",
>			  "Statement": [
>				{
>				  "Sid": "",
>				  "Effect": "Allow",
>				  "Principal": {
>					"Service": [
>					  "codedeploy.us-west-2.amazonaws.com",
>					  "codedeploy.us-east-1.amazonaws.com"
>					]
>				  },
>				  "Action": "sts:AssumeRole"
>				}
>			  ]
>			}

2.3.) The above role will be used while creating new Application deployment under codedeploy.
			"Select an existing service role that grants AWS CodeDeploy access to the instances - Service Role ARN*".

2.4.) Create Custom Policy for EC2 instance with the below template (ec2role).

>			{ 
>				"Version": "2012-10-17", 
>				"Statement": [   
>				  {     
>					  "Action": [       
>						  "s3:Get*",       
>						  "s3:List*"     
>					  ],     
>					  "Effect": "Allow",     
>					  "Resource": "*"   
>				  } 
>				]
>			}

3.) Launch  EC2 instance with ec2role.

4.) Run the below command as root in newly created EC2 instance.

>	yum -y update
>	yum install -y aws-cli
>	yum install tomcat6
>	cd /home/ec2-user

5.) To install Code Deploy Agent into EC2 instance run the below command.

>	aws s3 cp s3://aws-codedeploy-us-east-1/latest/install . --region us-east-1
>	chmod +x ./install

6.)  Quick hack to get the agent running faster.

> 	sed -i "s/sleep(.*)/sleep(10)/" install
>	./install auto

7.) Verify the code deploy agent is running.

>	service codedeploy-agent status

8.) Select Codedeploy and Create New Application.

8.1.)	Provide Application Name and Deployment Group Name.

8.2.)	Provide any key and value which is same for all EC2 instances used for deployment of this application.

8.3.)	Provide the type of deployment config and Service Role ARN as role-codedeploy.

8.4.)	Select Deploy New revision and Select Git for deployment and proceed with deploy.

#Reference Video.

>https://www.youtube.com/watch?v=qZa5JXmsWZs

#Reference Project.

>https://github.com/andrewpuch/code_deploy_example/
