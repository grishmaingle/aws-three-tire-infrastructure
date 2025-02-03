# aws-three-tire-infrastructure
This project sets up a Three-Tier Architecture on AWS, including a web server, application server, and RDS database. The infrastructure is manually configured for scalability, security, and high availability. It covers networking, security groups, and other essential AWS services to ensure a fully functional deployment. 🚀
------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------
Services that we are used :

1] Amazon EC2 Instances : These virtual servers host your web and application tiers.

2] Auto Scaling Groups (ASG): Automatically adjust the number of EC2 instances in response to traffic demands, ensuring optimal performance and cost efficiency.

3] Application Load Balancer (ALB): Distributes incoming client traffic across your web tier EC2 instances, enhancing availability and fault tolerance.

4] Amazon S3: Stores static assets like images, videos, and other media files, enabling efficient content delivery.

5]Amazon EFS: Provides a scalable file storage solution accessible by multiple EC2 instances, facilitating shared access to data.

6] Amazon RDS (Aurora MySQL): Manages your relational database, offering high availability and scalability for data storage and retrieval.

7] Amazon VPC: Defines your virtual network environment, isolating resources and controlling network traffic.

8] Amazon CloudWatch: Monitors application performance and resource utilization, providing insights for optimization.

9] Amazon SNS: Facilitates communication between components, such as sending notifications or triggering workflows.

10] AWS CloudTrail: Records API calls and user activity, aiding in security analysis and compliance auditing.

11] Amazon Route 53: Manages DNS routing, directing user requests to the appropriate resources.
Amazon CloudFront: Delivers content with low latency by caching static assets at edge locations.

------------------------------------------------------------------------------------------------
Architecture Overview :

![image](https://github.com/user-attachments/assets/32fb3804-983f-4475-bc58-e7bb1a633c5f)

   ARCHITECTURE FLOWN :

Client Request: Users access your React.js website, which is served by Nginx web servers on EC2 instances.

Static Content Delivery: Static assets are retrieved from Amazon S3, ensuring fast and reliable access.

API Calls: API requests are forwarded by Nginx to the internal-facing Application Load Balancer.

Application Processing: The Application Load Balancer directs these requests to Node.js application instances.

Database Interaction: The application tier interacts with the Aurora MySQL database on Amazon RDS for data operations.

Response Delivery: Processed data is sent back through the application tier, load balancer, and web tier to the client.

SECURITY AND MONITORING

IAM: Manages access controls and permissions for AWS resources.

CloudWatch: Monitors application performance and resource utilization, providing insights for optimization.

CloudTrail: Records API calls and user activity, aiding in security analysis and compliance auditing.

HIGH AVAILABILITY AND SCALABILITY :

Auto Scaling Groups: Ensure that the number of EC2 instances adjusts automatically based on traffic demands.

Multi-AZ Deployment: Deploys RDS instances across multiple Availability Zones for fault tolerance.

CloudFront: Caches content at edge locations to reduce latency and improve user experience.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Creating 3 Tire Architecture & Integrating other AWS Resources

------------------------------------------------------------------------------------------------

STEP 1 : Download Code from my Github in your Local System

-----------------------------------------------------------------------------------------------

STEP 2 : Create Two S3 Buckets

![image](https://github.com/user-attachments/assets/564ea3f0-51c1-4df2-b6ad-236354754b40)

=> Create one S3 bucket for storing web-server & app-server code and uplode the application code to your S3 from your local system.
![image](https://github.com/user-attachments/assets/62a188a0-8da7-4b2c-883f-eddbe8792edd)


=> Create another S3 bucket for VPC flow logs.

-----------------------------------------------------------------------------------------------

Step 3: Create IAM Role with Policies

=> S3 read only.

=> SSM managed instance core.
![image](https://github.com/user-attachments/assets/0ccf679a-52e6-4de9-bcd6-1c20b65f20c0)

-----------------------------------------------------------------------------------------------

Step 4: Create VPC, Subnets, IGW, NAT-GW, RT
![image](https://github.com/user-attachments/assets/0122623d-6f25-4ca2-a22d-e780c5d0148c)
![image](https://github.com/user-attachments/assets/ac285621-31ec-40aa-ba1c-2a87c5150437)
![image](https://github.com/user-attachments/assets/bbf9bc2c-d26d-4630-83e8-8c233662a0e8)
![image](https://github.com/user-attachments/assets/24db0493-beec-4e0b-84a2-135ea683e3d0)
![image](https://github.com/user-attachments/assets/0257dde8-fb31-42e2-8267-5cbec21de8f3)

=> Enable auto-assign public IP for web-tier public subnets for both web-tier subnet
![image](https://github.com/user-attachments/assets/2c3b3b9b-f8a1-4333-82c8-381bca5d9c5e)

=> Create flow logs for VPC & use the S3 bucket created above.
![image](https://github.com/user-attachments/assets/88d6af7d-a29e-45ea-9ea4-55ee71750922)


-----------------------------------------------------------------------------------------------

Step 5: Create Security Groups 

=> External-Load-Balancer-SG --> HTTP (80): 0.0.0.0/0.
![image](https://github.com/user-attachments/assets/324321ed-2266-4ac7-80ce-aefe11b82cc2)

=> Web-Tier-SG --> HTTP --> Ext-LB-SG.
![image](https://github.com/user-attachments/assets/e3b9aaa6-7f0f-4f0e-a1ee-46e94f4d7ab4)

=> Internal-Load-Balancer-SG --> HTTP --> Web-Tier-SG.
![image](https://github.com/user-attachments/assets/656b09db-3e94-4965-99ec-986ff6afcdb3)

=> App-Tier-SG --> Port 4000 --> Internal-LB-SG.
![image](https://github.com/user-attachments/assets/c15705a1-74ea-43c1-bdac-4725df095f1a)

=> DB-Tier-SG --> MySQL (3306) --> App-Tier-SG.
![image](https://github.com/user-attachments/assets/dc282a53-d040-4254-8425-c1f7adc15b1e)

-----------------------------------------------------------------------------------------------

Step 6: Create DB Subnet Group & RDS

=> Create DB subnet group.
![image](https://github.com/user-attachments/assets/af9a544a-cc75-4abe-8a0e-a1abd38bf7c1)
![image](https://github.com/user-attachments/assets/ffff7eb4-14d1-4f89-8683-45dd7d48c91d)

=> Create RDS - Multi-AZ and select the DB-Tire-SG security group.
![image](https://github.com/user-attachments/assets/728858c4-cb38-481b-8c3c-3f1ac07f1572)
![image](https://github.com/user-attachments/assets/5395b6de-07a6-4cdb-83b9-f75767c53955)
![image](https://github.com/user-attachments/assets/e06ca11c-b821-4761-85e5-0e873d5d3d6f)

=> Place them in DB subnet group created above.

-----------------------------------------------------------------------------------------------

Step 7: Create SNS for both App server and web server.
![image](https://github.com/user-attachments/assets/29cbbe0b-a29a-4f07-82a0-baea060c6fd7)
![image](https://github.com/user-attachments/assets/c2cc7930-74cd-4318-8af9-d6f13d35cecf)

-----------------------------------------------------------------------------------------------

Step 8: Create Test App Server, Install Packages, Test Connections

=> Test App-Server Commands
![image](https://github.com/user-attachments/assets/f6bf2bdb-2889-4792-ac9b-61978eddc1d5)

=> Create AMI.
![image](https://github.com/user-attachments/assets/8165f707-56be-49a7-a99b-032bba1c9beb)

=> Create launch template using AMI.
![image](https://github.com/user-attachments/assets/02c64ac0-9719-43f4-8c02-2f8e66659951)

=> Create target group.
![image](https://github.com/user-attachments/assets/335ef4ee-459f-4308-ab2a-3b5a52cd281c)

=> Create internal load balancer.
![image](https://github.com/user-attachments/assets/50528553-3070-4ab0-8677-35e8fe335832)

=> Create autoscaling group.
![image](https://github.com/user-attachments/assets/83fcb37d-709e-40ad-8b29-947f0529fbc5)
![image](https://github.com/user-attachments/assets/8c2cdb4b-4d1b-47f3-9198-8cca23740139)

=> Edit nginx.conf file in local system by adding Internal-LB-DNS & upload the file in S3.
![image](https://github.com/user-attachments/assets/05794670-8dd6-42c0-ac09-3abf85cb06de)

-----------------------------------------------------------------------------------------------

Step 9: Create Test Web Server, Install Packages (Nginx, Node.js (React)), Test Connections

=> Test Web-Server Commands.
![image](https://github.com/user-attachments/assets/c9ec0c95-ff48-45b1-b4db-efaed32fad99)

=> Create AMI.
![image](https://github.com/user-attachments/assets/f5201853-df58-4567-92e5-a2c3c6155e83)

=> Create launch template using AMI.
![image](https://github.com/user-attachments/assets/dfb5500a-9ee9-48af-aa7e-8ec00f4d5df3)

=> Create target group.
![image](https://github.com/user-attachments/assets/a4189045-04fd-41d6-966f-c7f0a34682a0)

=> Create external load balancer.
![image](https://github.com/user-attachments/assets/5bea2afb-7da3-4aed-be61-0878171ff36a)

=> Create autoscaling group.
![image](https://github.com/user-attachments/assets/e6e13c0d-dc2a-49d4-8fcb-383dddd3e1e6)

-----------------------------------------------------------------------------------------------

Step 10: Add External-ALB-DNS Record in Route 53
![image](https://github.com/user-attachments/assets/b3dc5e2d-ed64-4b46-b473-4aaf23cb24b3)

-----------------------------------------------------------------------------------------------

Step 11: Create CloudWatch Alarms Along with SNS
![image](https://github.com/user-attachments/assets/5b35c298-9479-4d16-9dcb-38b856d2b674)
![image](https://github.com/user-attachments/assets/a0bc2338-cbd9-4451-8017-83ae9c334049)

-----------------------------------------------------------------------------------------------

Step 12: Create CloudTrail

![image](https://github.com/user-attachments/assets/6c139c97-3bd1-46cc-ab10-758c22033303)

-----------------------------------------------------------------------------------------------
Final output : 
![image](https://github.com/user-attachments/assets/9f13f519-0031-44e6-a37b-d545ed20c736)

-----------------------------------------------------------------------------------------------
Step 13: Deleting the Entire Infrastructure
=> Delete CloudFront.

=> Delete CloudWatch alarms.

=> Delete records from Route 53.

=> Delete load balancers, target groups, ASG, launch templates.

=> Delete security group.

=> Delete NAT gateway (it will take 5 mins).

=> Release elastic IP.

=> Delete VPC.

=> Delete RDS subnet group, RDS.





