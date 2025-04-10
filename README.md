### **Task 1: Create Launch Template**

1. **Log in to AWS Management Console:**
   - Open your AWS Management Console and log in.

2. **Navigate to EC2 Service:**
   - In the top search bar, type `EC2` and select it.

3. **Create Launch Template:**
   - On the left sidebar, under **Instances**, click **Launch Templates**.
   - Click on **Create launch template**.

4. **Configure Launch Template:**
   - **Launch Template Name**: Give your template a name, e.g., `MyLaunchTemplate`.
   - **AMI (Amazon Machine Image)**: Select an appropriate AMI (e.g., Amazon Linux 2 or Ubuntu) for your EC2 instances.
   - **Instance Type**: Choose an instance type like `t2.micro` (or any other depending on your needs).
   - **Key Pair**: Choose an existing key pair or create a new one for SSH access to instances.
   - **User Data**: (Optional) Add any necessary user data to configure the instance upon launch (e.g., installing packages).
     - Example for installing `stress` tool:
       ```bash
       #!/bin/bash
       sudo amazon-linux-extras install epel -y
       sudo yum install stress -y
       stress -c 4
       ```
   - Click **Create launch template** to save.

---

### **Task 2: Set Up Auto Scaling Group**

1. **Navigate to Auto Scaling Groups:**
   - In the AWS Management Console, go to the EC2 service.
   - In the left sidebar, under **Auto Scaling**, click **Auto Scaling Groups**.

2. **Create Auto Scaling Group:**
   - Click on the **Create Auto Scaling group** button.

3. **Select Launch Template:**
   - Choose **Use Launch Template** and select the template you created earlier (`MyLaunchTemplate`).

4. **Configure Auto Scaling Group Settings:**
   - **Group Name**: Give it a name (e.g., `MyAutoScalingGroup`).
   - **Desired Capacity**: Set the number of instances you want to launch initially (e.g., 2 instances).
   - **Minimum Capacity**: Set the minimum number of instances (e.g., 1).
   - **Maximum Capacity**: Set the maximum number of instances (e.g., 5).
   - **Network Settings**: Select the VPC and subnets for the instances to be launched into.

5. **Advanced Settings (Optional):**
   - Configure scaling policies, termination policies, and health checks based on your requirements.

6. **Create Auto Scaling Group:**
   - After filling out the settings, click **Create Auto Scaling group**.

---

### **Task 3: Configure Scaling Policies**

1. **Navigate to Scaling Policies:**
   - In your Auto Scaling group settings, go to the **Scaling policies** section.

2. **Create Scaling Policy:**
   - Click on **Create scaling policy**.
   - Choose the **Target tracking scaling policy** or **Step scaling policy**, depending on how you want to scale.
   
   For example:
   - **Scaling in (Reduce instances)**: Set to scale down if the CPU usage is below 40% for 5 minutes.
   - **Scaling out (Add instances)**: Set to scale up if the CPU usage is above 70% for 5 minutes.

3. **Define Metrics and Thresholds:**
   - Set thresholds for CPU utilization or other metrics based on which you want the scaling action to trigger (e.g., if average CPU utilization exceeds 80%).

4. **Save Scaling Policies**.

---

### **Task 4: Attach ALB to Auto Scaling Group**

1. **Navigate to Load Balancing Settings:**
   - In the Auto Scaling group configuration, go to the **Load balancing** section.

2. **Edit Load Balancer Settings:**
   - Click **Edit** to modify the load balancing settings.
   - Choose **Attach to an existing load balancer**.
   - Select the **Application Load Balancer (ALB)** that you want to associate with the Auto Scaling group.

3. **Configure Target Group:**
   - Make sure that the Auto Scaling group is connected to the correct target group associated with your ALB.
   - Ensure that health checks are properly configured for the target group to determine the health of the instances.

4. **Save Changes**.

---

### **Task 5: Test Auto Scaling**

1. **Generate Load:**
   - SSH into one of the running instances, and generate traffic to simulate high CPU utilization.
   - Run the following commands to install and use the `stress` tool:
     ```bash
     sudo amazon-linux-extras install epel -y
     sudo yum install stress -y
     stress -c 4  # Stress 4 CPU cores
     ```

2. **Monitor Auto Scaling:**
   - Go to the **EC2 Dashboard** and navigate to **Auto Scaling Groups**.
   - Check the **Desired Capacity** and **Actual Capacity** to see if the number of instances increases based on the load.
   - You should see additional instances launched if the scaling policies trigger.

---

### **Side Notes:**

- **Documentation**: As you work through the project, make sure to document the settings for your Launch Template, Auto Scaling group, scaling policies, and ALB configuration.
- **GitHub/GitLab**: Store your project documentation in a GitHub or GitLab repository for future reference. You can also version control your AWS CloudFormation templates or other automation scripts.
- **Reflection**: Auto Scaling is crucial for applications with fluctuating traffic. It optimizes resource usage and ensures high availability by automatically adjusting resources based on demand.

---

### **Conclusion:**

By completing this project, you'll have gained practical experience configuring Auto Scaling with an Application Load Balancer and Launch Template in AWS. This will help you build scalable and resilient applications in the cloud, making them more responsive to changes in traffic without manual intervention.
