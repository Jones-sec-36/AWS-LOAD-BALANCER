# AWS-LOAD-BALANCER
AWS LOAD BALANCER
## AIM
To use Elastic Load Balancing (ELB) and Auto Scaling services to load balance and automatically scale an AWS infrastructure.

## ALGORITHM
### Step 1: Create an AMI for Auto Scaling
Open the EC2 console, confirm that Web Server 1 is running (2/2 status checks passed), select the instance, and choose Actions → Image and templates → Create image. Name it "WebServerAMI" and create it. This AMI will be used to launch identical instances later.

### Step 2: Create a Target Group and Load Balancer
Create a Target Group named "LabGroup" (type: Instances, VPC: Lab VPC) without registering targets yet. Then create an Application Load Balancer named "LabELB" under Lab VPC, mapped to Public Subnet 1 and Public Subnet 2, using the Web Security Group, with the HTTP:80 listener forwarding to LabGroup.

### Step 3: Create a Launch Template and Auto Scaling Group
Create a Launch Template named "LabConfig" using the WebServerAMI, instance type t2.micro, key pair "vockey", the Web Security Group, and Detailed CloudWatch monitoring enabled. Using this template, create an Auto Scaling group named "Lab Auto Scaling Group" attached to Private Subnet 1 and Private Subnet 2, linked to the LabGroup target group, with desired/minimum/maximum capacity of 2/2/6 and a target tracking scaling policy set to maintain 60% average CPU utilization.

### Step 4: Verify Load Balancing
Confirm that two new "Lab Instance" EC2 instances were launched by Auto Scaling and that both show a "healthy" status in the LabGroup target group. Copy the Load Balancer's DNS name and open it in a browser to confirm the application is being served correctly through the load balancer.
### Step 5: Test Auto Scaling
Lower the scaling policy's target CPU value to 50% to make scaling trigger sooner, then use the application's "Load Test" feature to generate high CPU load across the instances. Monitor the CloudWatch alarms (AlarmLow/AlarmHigh) until AlarmHigh enters the "In alarm" state, then verify in the EC2 console that additional instances were automatically launched to handle the load.

### Step 6: Terminate the Original Web Server
Select Web Server 1 (the original instance used to create the AMI) and terminate it, since it is no longer needed once the Auto Scaling group is managing instances independently.

## OUTPUT
<img width="1920" height="1200" alt="Screenshot (249)" src="https://github.com/user-attachments/assets/4cbb1947-7942-4459-a482-36d6c3247d7a" />
<img width="1920" height="1200" alt="Screenshot (250)" src="https://github.com/user-attachments/assets/99d1cb26-b0b7-4a76-9da2-872b202443e2" />
<img width="1920" height="1200" alt="Screenshot (251)" src="https://github.com/user-attachments/assets/257afc6d-75c5-4296-89e9-c0ffbab8aba8" />
<img width="1920" height="1200" alt="Screenshot (252)" src="https://github.com/user-attachments/assets/b665d45e-8337-4806-bf9b-7db6535dbe63" />
<img width="1920" height="1200" alt="Screenshot (253)" src="https://github.com/user-attachments/assets/047f1421-4542-4eb5-9625-beb8c500460f" />
<img width="1920" height="1200" alt="Screenshot (254)" src="https://github.com/user-attachments/assets/a4d3b5ba-27f3-4d46-a957-c09f00b674a7" />
<img width="1920" height="1200" alt="Screenshot (255)" src="https://github.com/user-attachments/assets/9616345e-b5c2-4960-ac9a-4812d1e549b8" />
<img width="1920" height="1200" alt="Screenshot (257)" src="https://github.com/user-attachments/assets/096ba578-a31e-4ce2-93cb-8f777a6e860e" />
<img width="1920" height="1200" alt="Screenshot (259)" src="https://github.com/user-attachments/assets/7be06ed2-9331-41f7-b283-5ce5c4d7c8d6" />
<img width="1920" height="1200" alt="Screenshot (258)" src="https://github.com/user-attachments/assets/08940f20-e6a3-4bfe-9c9c-cb659eb4825a" />
<img width="1915" height="928" alt="637671314-12445bd5-3bc7-4649-845c-ed365fb60a1f" src="https://github.com/user-attachments/assets/d195eb50-5a1c-4982-bf22-6887130861a5" />

## RESULT
Thus, an AMI was created from a running EC2 instance, a Load Balancer was configured to distribute traffic across multiple instances, an Auto Scaling group was set up with a target tracking scaling policy, and the infrastructure was verified to automatically scale out under increased load using CloudWatch alarms.
