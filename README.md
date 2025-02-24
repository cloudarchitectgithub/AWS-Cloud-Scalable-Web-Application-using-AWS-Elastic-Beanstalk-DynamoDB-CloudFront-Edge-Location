# AWS Cloud - Implementation of a Scalable Web Application using AWS Elastic Beanstalk, DynamoDB, CloudFront, and Edge Location

<p align="center">
  <img src="https://i.imgur.com/ahPBgKb.png" height="80%" width="80%" alt="Scalable Web Application Banner"/>
</p>

## Project Description
In this project, we are going to make an application that needs to support the high demand of a large number of users accessing it simultaneously. This application will be used in The Cloud Bootcamp Conference, which is a conference that will have participants from all over the world. More than 10,000 people are expected to participate, both in-person and online. To achieve this, the architecture was required to scale automatically based on real-time demand, ensuring system availability and responsiveness during traffic spikes. At some point during the event, 10 vouchers will be drawn for 3 Cloud certifications. The audience will register their email to guarantee participation in the raffle.

## Project Objectives
The primary objectives of this project were to:
1. **Design a Scalable Architecture:** Create an AWS-based architecture capable of handling high traffic loads, particularly during a virtual cloud conference event with over 10,000 simultaneous users. The system should be able to scale dynamically to manage varying traffic levels effectively.
2. **Implement Auto-Scaling:** Configure auto-scaling policies to automatically adjust the number of EC2 instances based on real-time CPU utilization. This ensures that the application can handle peak loads without manual intervention, maintaining performance and availability.
3. **Ensure High Availability and Fault Tolerance:** Utilize Elastic Load Balancer (ELB) and CloudFront to distribute traffic and optimize content delivery across multiple regions. This setup should enhance the application’s responsiveness and reliability by preventing any single point of failure.
4. **Monitor and Manage Performance:** Use CloudWatch to monitor the health and performance of the application. Set up alarms and triggers to respond to metrics such as CPU utilization, ensuring that scaling actions are taken as needed to maintain optimal performance.
5. **Facilitate User Registration:** Leverage DynamoDB to store user registration data efficiently, ensuring that the system can handle a high volume of concurrent user registrations during the event.
6. **Demonstrate Load Handling Capabilities:** Conduct stress tests to validate that the system can handle substantial traffic and simulate real-world conditions, ensuring that the architecture performs as expected under high load scenarios.

---

## Project Solution

### Part 1: Initial Setup
1. **Create a DynamoDB Table:** Set up a DynamoDB table to store participant email addresses, which will be used to manage user registrations during the event.
2. **Deploy Application with Elastic Beanstalk:** Use AWS Elastic Beanstalk to deploy the application. Elastic Beanstalk provisions the necessary EC2 instances and sets up an Elastic Load Balancer (ELB) to manage incoming traffic.

### Part 2: Auto-Scaling Configuration
1. **Configure Auto-Scaling:** Set up auto-scaling rules within Elastic Beanstalk. The rules are configured to add a new EC2 instance when CPU utilization exceeds 50%, ensuring that the system can handle increased load.
2. **Monitor Auto-Scaling:** Use CloudWatch to monitor the performance and health of the EC2 instances. Set up CloudWatch alarms to trigger auto-scaling actions when necessary.

### Part 3: Load Testing
1. **Simulate Load:** Use the stress utility on an EC2 instance to simulate high CPU load. This step tests the auto-scaling functionality by pushing the CPU utilization beyond 50%.
2. **Monitor Scaling Actions:** Observe the auto-scaling process as it triggers the addition of a new EC2 instance in response to high CPU utilization. Verify that the new instance is properly registered with the Elastic Load Balancer and CloudFront.

### Part 4: Verification
1. **Verify Functionality:** Access the application via the CloudFront URL and ensure that new user registrations are successfully processed and stored in DynamoDB.
2. **Stop Load Testing:** Terminate the stress process to observe the system's scaling down behavior. Confirm that the auto-scaling group reduces the number of instances back to the baseline level as traffic normalizes.

---

## Tools and Technologies
- **Amazon Web Services (AWS):**
  - **Elastic Beanstalk:** To simplify application deployment and manage the underlying infrastructure.
  - **EC2 Instances:** Virtual servers providing computing capacity to host the application.
  - **Auto Scaling:** Automatically increases or decreases the number of EC2 instances in response to traffic or CPU load.
  - **Elastic Load Balancer (ELB):** Distributes incoming application traffic across multiple EC2 instances to ensure high availability.
  - **CloudFront:** A Content Delivery Network (CDN) to speed up content delivery and provide global access to the application.
  - **DynamoDB:** A NoSQL database used to store user registration data.
  - **Route 53:** Manages the DNS for the application, routing traffic efficiently.
  - **CloudWatch:** Monitors the system and triggers alarms when metrics (e.g., CPU utilization) exceed predefined thresholds.
- **Linux Utilities:**
  - **Stress:** A Linux-based tool used to simulate high CPU loads during testing.

---

## Step-by-Step Directions

### Step 1: Login to the AWS Management Console
1. Open your web browser and navigate to the [AWS Management Console](https://console.aws.amazon.com/).
2. Enter your AWS Account credentials to log in.

<p align="center">
  <img src="https://i.imgur.com/SMKik9p.png" height="80%" width="80%" alt="AWS Login Screenshot"/>
</p>

### Step 2: Create DynamoDB Table
1. Go to DynamoDB in the AWS Console.
2. Create a new table named "users" with the partition key as "email."

<p align="center">
  <img src="https://i.imgur.com/rhcxeFs.png" height="80%" width="80%" alt="DynamoDB Table Screenshot"/>
</p>

### Step 3: Deploy Application using Elastic Beanstalk
1. Go to Elastic Beanstalk in the AWS Console.
2. Create a new application named "TCB-Conference."
3. Configure the application with Python 3.7, or 3.8 if 3.7 is not available.
4. Upload the application code (zip file) or provide a public S3 URL.
5. Configure more options:
   - Set high availability.
   - Add the environment variable `AWS_REGION` with the value of the desired AWS region.

<p align="center">
  <img src="https://i.imgur.com/TqLyRH0.png" height="80%" width="80%" alt="Elastic Beanstalk Configuration Screenshot"/>
</p>

6. Change the root volume type to General Purpose (SSD) with a size of 10 GB.
7. Set the minimum instances to 2 and maximum instances to 4.
8. Set instance type to `t2.micro`.
9. Configure scaling triggers for CPU utilization (50% upper threshold, 40% lower threshold).

<p align="center">
  <img src="https://i.imgur.com/2dwRxHY.png" height="80%" width="80%" alt="Auto Scaling Configuration Screenshot"/>
</p>

10. Set the EC2 key pair.
11. Update networking settings (use default VPC, public load balancer, associate public IP address).
12. Review and create the application.

### Step 4: Troubleshoot Application Error
1. Check Elastic Beanstalk logs for errors.
2. Identify and resolve the "Access Denied" exception for DynamoDB.
3. Update the EC2 role associated with Elastic Beanstalk with the necessary DynamoDB permissions.

### Step 5: Verify Application Functionality
1. Test the application to ensure successful registration of email addresses.

<p align="center">
  <img src="https://i.imgur.com/cmm3IKL.png" height="80%" width="80%" alt="Application Test Screenshot"/>
</p>

2. Check DynamoDB to verify test entries.

<p align="center">
  <img src="https://i.imgur.com/k0HO0EX.png" height="80%" width="80%" alt="DynamoDB Entries Screenshot"/>
</p>

### Step 6: Deploying Amazon CloudFront Distribution
1. Open the AWS Management Console and search for "CloudFront."
2. Click on the CloudFront service to access the main screen.
3. Click the orange "Create Distribution" button to initiate the CloudFront distribution creation process.
4. Choose the Elastic Load Balancer (ELB) as the source for your application.

<p align="center">
  <img src="https://i.imgur.com/60UFZ9h.png" height="80%" width="80%" alt="CloudFront ELB Source Screenshot"/>
</p>

5. Maintain the default origin settings; no modifications needed for the origin path.
6. Under "Allowed HTTP Methods," change it to the third option to enable the POST method for data search in DynamoDB.
7. Scroll down to the settings area. Choose the appropriate price class based on your application's global usage.

<p align="center">
  <img src="https://i.imgur.com/Wf7VSiR.png" height="80%" width="80%" alt="CloudFront Price Class Screenshot"/>
</p>

8. Optionally, configure a custom domain name for a branded URL. For this example, use the default CloudFront domain.
9. Click "Create Distribution" to start the CloudFront distribution creation. This process may take up to 10 minutes.
10. Once completed, the CloudFront distribution will have an associated domain name. Copy the domain name for accessing the application.

<p align="center">
  <img src="https://i.imgur.com/Icgp1Hv.png" height="80%" width="80%" alt="CloudFront Domain Screenshot"/>
</p>

11. Verify that the application loads successfully using the CloudFront URL. All static and dynamic files are now cached in CloudFront edge locations for improved user performance.

<p align="center">
  <img src="https://i.imgur.com/plslQdQ.png" height="80%" width="80%" alt="CloudFront Application Screenshot"/>
</p>

### Step 7: Loading Tests and Auto Scaling
1. Access Elastic Beanstalk in the AWS console.
2. Explore the Elastic Beanstalk deployment and configuration.
3. Navigate to the "Configuration" tab and observe the configured CPU utilization threshold for auto scaling.

<p align="center">
  <img src="https://i.imgur.com/dPsHU1S.png" height="80%" width="80%" alt="Auto Scaling Configuration Screenshot"/>
</p>

4. Check the health and instances under the Elastic Beanstalk environment.
5. Use the stress-testing utility to simulate high CPU utilization on one of the EC2 instances.
6. Observe the Elastic Beanstalk environment status changing to "Warning" due to increased CPU utilization.
7. View events and notice the auto scaling group taking action to add a new EC2 instance.

<p align="center">
  <img src="https://i.imgur.com/NxMhGVk.png" height="80%" width="80%" alt="Auto Scaling Events Screenshot"/>
</p>

8. Check the EC2 dashboard to confirm the addition of a new instance.
9. Copy the IP Address of an EC2 Instance:
   - Open Git Bash on your local machine.
   - Navigate to the directory containing the private key of the EC2 instance.
   - Connect to the EC2 Instance:

<p align="center">
  <img src="https://i.imgur.com/Ktj17i0.png" height="80%" width="80%" alt="SSH to EC2 Screenshot"/>
</p>

10. Install Stress Testing Utility:

<p align="center">
  <img src="https://i.imgur.com/j0afp0L.png" height="80%" width="80%" alt="Install Stress Utility Screenshot"/>
</p>

11. Run Stress Command:
    - Run the stress command to increase the CPU utilization on the EC2 instance.

<p align="center">
  <img src="https://i.imgur.com/Y2nao3m.png" height="80%" width="80%" alt="Run Stress Command Screenshot"/>
</p>

12. Observe Elastic Beanstalk Status:
    - Check the Elastic Beanstalk console to observe the status change to "warning" due to increased CPU utilization.

<p align="center">
  <img src="https://i.imgur.com/I3JgrZY.png" height="80%" width="80%" alt="Elastic Beanstalk Warning Screenshot"/>
</p>

13. Monitor EC2 Instance:
    - Open a new session to the stressed EC2 instance to monitor its CPU utilization. Use the `top` command to observe the 100% CPU utilization.

<p align="center">
  <img src="https://i.imgur.com/apF69bg.png" height="80%" width="80%" alt="EC2 CPU Utilization Screenshot"/>
</p>

14. Observe Autoscaling Process:
    - Monitor the Elastic Beanstalk and EC2 console to see the autoscaling process adding a new instance due to the increased CPU utilization.

<p align="center">
  <img src="https://i.imgur.com/Ogq47mW.png" height="80%" width="80%" alt="Autoscaling Process Screenshot"/>
</p>

15. Stop Loading Process:
    - On the EC2 instance running the stress test, stop the process by pressing `Ctrl + C`.
    - Check if the stress process is not running anymore using the command:

<p align="center">
  <img src="https://i.imgur.com/BEIiBDv.png" height="80%" width="80%" alt="Stop Stress Process Screenshot"/>
</p>

16. Check Elastic Beanstalk Health:
    - Observe that the Elastic Beanstalk health status transitions from "warning" to "okay" after the stress test stops.

<p align="center">
  <img src="https://i.imgur.com/l9ZfMA0.png" height="80%" width="80%" alt="Elastic Beanstalk Health Screenshot"/>
</p>

17. Access the CloudFront URL, add a new registration to the application.

<p align="center">
  <img src="https://i.imgur.com/u6YBwDP.png" height="80%" width="80%" alt="CloudFront Application Screenshot"/>
</p>

<p align="center">
<img src="https://i.imgur.com/Q5X9sTd.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

18. Verify the addition of the new registration in DynamoDB.

<p align="center">
  <img src="https://i.imgur.com/KdLURjY.png" height="80%" width="80%" alt="DynamoDB Entries Screenshot"/>
</p>

19. Observe the health status changing back to "OK" in Elastic Beanstalk.
20. Wait for the auto scaling group to scale down and remove the additional EC2 instance.
21. Environment in a healthy state.

<p align="center">
  <img src="https://i.imgur.com/BJ6EyAS.png" height="80%" width="80%" alt="Healthy Environment Screenshot"/>
</p>

---

## Project Conclusion
The project successfully demonstrated the effectiveness of AWS Elastic Beanstalk in deploying and managing a scalable architecture for high-traffic cloud events. The system handled simulated load increases efficiently, scaling up when CPU usage exceeded thresholds, and ensuring minimal downtime. By the end of testing, the architecture was able to support significant loads, distributing traffic effectively across multiple EC2 instances via the Elastic Load Balancer.

---

## Challenges Encountered
1. **Setting Up Auto Scaling Triggers:** Initially, it was challenging to configure the auto-scaling policies to scale at the appropriate thresholds without over-provisioning. Finding the right balance between cost-effectiveness and performance took several iterations.
2. **Load Testing Accuracy:** During the stress test, ensuring the load properly simulated real-world user behavior was another challenge. Fine-tuning the test required adjustments to parameters to achieve realistic results.
3. **Managing Costs:** Managing the cost of running multiple EC2 instances, especially during long tests, became a challenge. AWS billing for resources during peak loads had to be carefully monitored.

---

## Lessons Learned
1. **Effective Scaling is Key to Performance:** The project highlighted the importance of configuring auto-scaling rules effectively. Too aggressive scaling could lead to unnecessary costs, while delayed scaling could cause performance bottlenecks.
2. **Elastic Beanstalk Simplifies Management:** AWS Elastic Beanstalk proved to be a powerful tool for managing infrastructure with minimal hands-on intervention, allowing for smooth scaling and traffic management.
3. **CloudFront Increases Global Responsiveness:** Incorporating CloudFront made a noticeable difference in global access speed, reducing latency for users across different regions.
4. **Monitoring is Essential:** AWS CloudWatch's monitoring and alerting capabilities provided crucial insights into system performance, helping to optimize auto-scaling decisions and maintain high availability.

---

## Future Improvements
1. **Optimize Cost Efficiency:** Further refinement of auto-scaling policies to reduce costs without sacrificing performance could be explored. This includes using more advanced monitoring tools and scaling based on metrics like memory utilization alongside CPU.
2. **Use of Spot Instances:** For non-critical operations, incorporating spot instances could further reduce costs while maintaining system availability during high demand.
3. **Explore Serverless Architectures:** While EC2 provided reliable computing power, future iterations could explore the use of AWS Lambda to reduce infrastructure overhead and improve scalability.
4. **Automated Testing Framework:** Implementing automated load testing frameworks (e.g., using AWS Load Testing tools) could provide more accurate and repeatable stress testing scenarios, leading to better insights into system performance under load.
