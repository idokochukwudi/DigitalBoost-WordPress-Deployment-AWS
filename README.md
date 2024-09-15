# DigitalBoost-WordPress-Deployment-AWS

## Introduction
This project focuses on deploying a high-performance, scalable WordPress website using AWS cloud infrastructure for DigitalBoost, a digital marketing agency. The project aims to leverage various AWS services like VPC, EC2, RDS, EFS, and Application Load Balancer (ALB) to ensure secure, reliable, and efficient hosting of the website.

## Objectives
1. VPC Setup:
Isolate and secure WordPress infrastructure.
2. Subnets:
Create public and private subnets across multiple availability zones for redundancy.
3. Internet and NAT Gateway:
Enable internet access for public subnets and secure private subnet internet access.
4. Route Tables:
Configure public and private subnets to route traffic efficiently.
5. Bastion Host:
Set up a secure SSH access point to the private instances.
6. WordPress EC2 Instance:
Deploy WordPress on EC2 instances.
7. EFS:
Use Amazon EFS to store WordPress files for scalable and shared access.
8. MySQL RDS:
Set up a managed MySQL database for WordPress data storage.
9. Load Balancer:
Use an Application Load Balancer for distributing traffic to multiple EC2 instances.

## 1. VPC Setup
- **Objective:** Isolate and secure the WordPress infrastructure. 
- Steps:
1. Define an IP address range for the VPC (e.g., 10.0.0.0/16).
2. Create the VPC.
![](./1.%20VPC%20Setup/1.VPC-name-cidr-block.png)
![](./1.%20VPC%20Setup/2.click-create-VPC.png)

### 3. Create two public subnets and two private subnets across different Availability Zones for redundancy.
![](./1.%20VPC%20Setup/1.Create%20Subnets-PUBLIC-AND-PRIVATE/1.choose-VPC.png)
![](./1.%20VPC%20Setup/1.Create%20Subnets-PUBLIC-AND-PRIVATE/2.public-subnet1-config.png)
![](./1.%20VPC%20Setup/1.Create%20Subnets-PUBLIC-AND-PRIVATE/3.public-subnet2-config.png)
![](./1.%20VPC%20Setup/1.Create%20Subnets-PUBLIC-AND-PRIVATE/4.private-subnet1-config.png)
![](./1.%20VPC%20Setup/1.Create%20Subnets-PUBLIC-AND-PRIVATE/5.private-subnet2-config-click-create-subnet.png)
![](./1.%20VPC%20Setup/1.Create%20Subnets-PUBLIC-AND-PRIVATE/6.subnets-created.png)

## 4. Create Internet Gateway and NAT Gateway

**Objective:** The objective is to set up an Internet Gateway for public subnet instances to access the internet and a NAT Gateway to provide secure outbound internet access for private subnet instances, ensuring efficient and secure VPC connectivity.
### - Create an Internet Gateway (IGW)
- Provide a name for the Internet Gateway
- Click Create internet gateway.
- Attach the Internet Gateway to Your VPC
![](./1.%20VPC%20Setup/2.Create%20internet%20gw%20and%20NAT%20gw/1.Internet-gateway/1.internet-gateway-name.png)
![](./1.%20VPC%20Setup/2.Create%20internet%20gw%20and%20NAT%20gw/1.Internet-gateway/2.click-actions-attach-to-VPC.png)
![](./1.%20VPC%20Setup/2.Create%20internet%20gw%20and%20NAT%20gw/1.Internet-gateway/3.select-digitalboost-VPC-click-attach.png)

### - Create a NAT Gateway
- Click Create NAT gateway.
- Choose the public subnet where the NAT Gateway will reside.
- Click Allocate Elastic IP
- Click Create NAT gateway.
![](./1.%20VPC%20Setup/2.Create%20internet%20gw%20and%20NAT%20gw/2.NAT%20Gateway/1.nat-gateway-config.png)
![](./1.%20VPC%20Setup/2.Create%20internet%20gw%20and%20NAT%20gw/2.NAT%20Gateway/2.NAT-Created.png)

## 5. Configure route tables:
- Associate the public subnets with a route table that routes traffic to an Internet Gateway.
![](./1.%20VPC%20Setup/3.Configure%20Route%20Tables/1.Public-RouteTable/1.public-subnet-RT-config.png)
![](./1.%20VPC%20Setup/3.Configure%20Route%20Tables/1.Public-RouteTable/2.edit-subnet-association.png)
![](./1.%20VPC%20Setup/3.Configure%20Route%20Tables/1.Public-RouteTable/3.select-public-subnets-click-save-associations.png)
![](./1.%20VPC%20Setup/3.Configure%20Route%20Tables/1.Public-RouteTable/4.click-edith-route.png)
![](./1.%20VPC%20Setup/3.Configure%20Route%20Tables/1.Public-RouteTable/5.click-add-route.png)
![](./1.%20VPC%20Setup/3.Configure%20Route%20Tables/1.Public-RouteTable/6.igw-route.png)

- Associate the private subnets with a route table that routes traffic to a NAT Gateway.
![](./1.%20VPC%20Setup/3.Configure%20Route%20Tables/2.Private-RouteTable/1.private-subnet-RT-config.png)
![](./1.%20VPC%20Setup/3.Configure%20Route%20Tables/2.Private-RouteTable/2.edit-subnet-associations.png)
![](./1.%20VPC%20Setup/3.Configure%20Route%20Tables/2.Private-RouteTable/3.select-private-subnets.png)
![](./1.%20VPC%20Setup/3.Configure%20Route%20Tables/2.Private-RouteTable/4.click-routes-edit-routes.png)
![](./1.%20VPC%20Setup/3.Configure%20Route%20Tables/2.Private-RouteTable/5.click-add-route.png)
![](./1.%20VPC%20Setup/3.Configure%20Route%20Tables/2.Private-RouteTable/6.route-via-nat-gateway.png)

## 6. Launch Bastion Host
**Objective:** Set up a secure access point for SSH into your private instances.

STEPS: 
### 1. Create Security Group for Bastion Host:
- Allow inbound SSH (port 22) from your IP address.
- Allow outbound traffic to anywhere.
![](./5.%20Launch%20the%20Bastion%20Host/security-group/click-create.png)
![](./5.%20Launch%20the%20Bastion%20Host/security-group/SG-config.png)
### 2. Launch Bastion Host
- Use Amazon Linux 2 AMI.
- Place it in the public subnet.
- Assign security group for the bastion host.
- Ensure the instance has a public IP.
![](5.%20Launch%20the%20Bastion%20Host/1.name-and-ami.png)
![](./5.%20Launch%20the%20Bastion%20Host/2.keypair-edit.png)
![](./5.%20Launch%20the%20Bastion%20Host/3.networking-click-launch.png)

## - Open Terminal (Example: Gitbash)
- Transfer-keypair to bastion host
- SSH into bastion host
- Verify transfer
- set-permission
![](./5.%20Launch%20the%20Bastion%20Host/4.transfer-keypair-to-bastion-host.png)
![](./5.%20Launch%20the%20Bastion%20Host/5.ssh-into-bastion-host.png)
![](./5.%20Launch%20the%20Bastion%20Host/6.verify-transfer.png)
![](./5.%20Launch%20the%20Bastion%20Host/7.set-permission.png)

## 7. Launch WordPress EC2 Instance
**Objective:** Set up a WordPress application on a private subnet.

STEPS:
### 1. Create Security Group for WordPress EC2 Instance:
1. Create a Security Group for Private Instances:
- Go to the EC2 Dashboard.
- Select Security Groups from the left-hand menu.
- Click Create Security Group.
- Name: e.g "PrivateInstanceSG"
- Description: Security group for private instances accessed via Bastion Host.
- VPC: Select the appropriate VPC.
2. Configure Inbound Rules for SSH:
- In the Inbound rules section, add the following rule:
    - Type: SSH
    - Protocol: TCP
    - Port Range: 22
    - Source: Choose the security group ID of the BastionHostSG (This allows SSH access only from the Bastion Host).
3. Outbound Rule:
    - Allow all outbound traffic.
- Click Create security group.
![](./2.%20Launch%20WordPress%20Instance/security-group/1.ec2-sg.png)
![](./2.%20Launch%20WordPress%20Instance/security-group/2.click-create.png)

## 8. Launch the EC2 Instance for WordPress
**Objective:** Set up an EC2 instance for the initial WordPress installation.
STEPS:
- Go to EC2 Dashboard > Launch Instance.
- **AMI:** Choose Amazon Linux 2 AMI.
![](./2.%20Launch%20WordPress%20Instance/1.name-and-ami.png)
- **Instance Type:** Choose an appropriate type (e.g., t2.micro).
- **Network:** Select DigitalBoostVPC.
- **Subnet:** Select Private-Subnet.
- **Security Group:** Select EC2SecurityGroup which allows inbound traffic on ports 22 (SSH), 80 (HTTP), and 443 (HTTPS).
- **Key Pair:** Select or create a key pair.
- Launch the instance.
![](./2.%20Launch%20WordPress%20Instance/2.keypair-edit.png)
![](./2.%20Launch%20WordPress%20Instance/3.networking.png)
![](./2.%20Launch%20WordPress%20Instance/4.click-launch.png)

## 9. EFS Setup for WordPress Files
**Objective:** Utilize Amazon Elastic File System (EFS) to store WordPress files for scalable and shared access.

STEPS:

1. Create an EFS File System:

- Navigate to the EFS Dashboard.
- Click Create File System.
- Attach it to your VPC, ensuring the security group allows NFS traffic (default port: 2049).
![](./4.%20Create%20the%20Elastic%20File%20System%20EFS/1.name-and-vpc.png)
![](./4.%20Create%20the%20Elastic%20File%20System%20EFS/2.efs-created.png)
- Create mount targets in each private subnet.
![](./4.%20Create%20the%20Elastic%20File%20System%20EFS/Create%20mount%20targets%20for%20each%20Availability%20Zone/1.click-on-file-system-ID.png)
![](./4.%20Create%20the%20Elastic%20File%20System%20EFS/Create%20mount%20targets%20for%20each%20Availability%20Zone/2.click-network-manage.png)
![](./4.%20Create%20the%20Elastic%20File%20System%20EFS/Create%20mount%20targets%20for%20each%20Availability%20Zone/3.save.png)

## 10. Mount the EFS on WordPress Instances:
1.  SSH into Bastion Host:
    - Connect to the Bastion Host (Public Subnet): First, SSH into the Bastion Host, which is in the public subnet. This host has a public IP address that you can access directly from your local machine.

        `ssh -i /path/to/your-key.pem ec2-user@<Bastion-Host-Public-IP>`
    - Replace `/path/to/your-key.pem` with the path to your private key file.
    - Replace `<Bastion-Host-Public-IP>` with the actual public IP address of your Bastion Host.

![](./5.%20Launch%20the%20Bastion%20Host/5.ssh-into-bastion-host.png)
2. SSH from the Bastion Host to the WordPress EC2 Instance (Private Subnet): Once you're logged into the Bastion Host, you can SSH into the WordPress EC2 instance in the private subnet.

    `ssh -i /path/to/your-key.pem ec2-user@<WordPress-Private-IP>`

- Replace `/path/to/your-key.pem` with the path to your private key file.
- Replace `<WordPress-Private-IP>` with the private IP address of your WordPress EC2 instance.
![](./2.%20Launch%20WordPress%20Instance/ssh/1.copy-private-ip.png)
![](./2.%20Launch%20WordPress%20Instance/ssh/2.SSH-into-private-instance-via-bastion-host.png)

3. Install EFS Utilities on Your EC2 Instances
- On Amazon Linux 2, use the following command to install the amazon-efs-utils package: `sudo yum install -y amazon-efs-utils`
- Create a Mount Directory
![](./4.%20Create%20the%20Elastic%20File%20System%20EFS/Mount%20EFS%20on%20EC2%20Instances%20and%20Configure%20WordPress/1.create-a-mount-directory.png)
4. Mount the EFS File System to the EC2 Instance
- Go to the EFS dashboard and copy the `NFS Client` for the EFS you just created.
![](./4.%20Create%20the%20Elastic%20File%20System%20EFS/Mount%20EFS%20on%20EC2%20Instances%20and%20Configure%20WordPress/2.copy-NFS-client.png)
- Replace `efs` with Mounth Directory
![](./4.%20Create%20the%20Elastic%20File%20System%20EFS/Mount%20EFS%20on%20EC2%20Instances%20and%20Configure%20WordPress/3.replace-efs-with-mount-directory.png)
- Mount the EFS File System and Verify the Mount
![](./4.%20Create%20the%20Elastic%20File%20System%20EFS/Mount%20EFS%20on%20EC2%20Instances%20and%20Configure%20WordPress/4.Mount-EFS-and-verify.png)
- Edit the /etc/fstab file to ensure the EFS mounts automatically on instance reboot. `sudo nano /etc/fstab`
- Add the following line: `<EFS_ID>:/ /var/www/html efs defaults,_netdev 0 0`
- Save and exit the file.
![](./4.%20Create%20the%20Elastic%20File%20System%20EFS/Mount%20EFS%20on%20EC2%20Instances%20and%20Configure%20WordPress/5.auto-mount-config.png)


## 11. Install and Configure WordPress
- Run the following command to install Apache, PHP, MySQL
![](./2.%20Launch%20WordPress%20Instance/Install%20Apache,%20MySQL,%20and%20PHP/1.Install-Apache-MySQL-and-PHP.png)
![](./2.%20Launch%20WordPress%20Instance/Install%20Apache,%20MySQL,%20and%20PHP/Install-APACHE/1.install.png)
- After installation, start and enable the service:
![](./2.%20Launch%20WordPress%20Instance/Install%20Apache,%20MySQL,%20and%20PHP/Install-APACHE/2.enable-start.png)

- Install WordPress on EC2 Instances:
![](./2.%20Launch%20WordPress%20Instance/Install-WordPress/1.Install-WordPress.png)
![](./2.%20Launch%20WordPress%20Instance/Install-WordPress/1.png)
![](./2.%20Launch%20WordPress%20Instance/Install-WordPress/2..png)


## 12. WS MySQL RDS Setup
**Objective:** Deploy a managed MySQL database using Amazon RDS for WordPress data storage.

STEPS:
1. Configure Security Groups:
- Create a security group for the RDS instance allowing inbound traffic on MySQL port 3306 from the WordPress EC2 instances.
![](./3.%20AWS%20MySQL%20RDS%20Setup/Security-Group/1.sg-config.png)
![](./3.%20AWS%20MySQL%20RDS%20Setup/Security-Group/2.click-create.png)

2. Create an RDS Instance:
- Choose the MySQL engine and configure instance details like storage, instance class, and Multi-AZ deployment for high availability.
![](./3.%20AWS%20MySQL%20RDS%20Setup/1.select-MySQL.png)
![](./3.%20AWS%20MySQL%20RDS%20Setup/2.free-tier.png)
![](./3.%20AWS%20MySQL%20RDS%20Setup/3.instance-name-admin-password.png)
![](./3.%20AWS%20MySQL%20RDS%20Setup/4.vpc.png)
![](./3.%20AWS%20MySQL%20RDS%20Setup/5.%20sg-az.png)
![](./3.%20AWS%20MySQL%20RDS%20Setup/6.%20click-create-database.png)

## 13. Connect WordPress to RDS:
1. Rename the default configuration file
- Rename the default configuration file (typically wp-config-sample.php) to wp-config.php.
![](./2.%20Launch%20WordPress%20Instance/Configure%20WordPress%20to%20Connect%20to%20RDS/1.Rename-the-default-configuration-file.png)
2. Open wp_config.php for editing
- First, you need to open the `wp_config.php` file for editing so you can make changes.
3. Enter your DB details as supplied during your RDS setup
- After opening the file, enter the database details (DB name, username, password, and host) as provided during the setup of your RDS instance.
4. Save and exit
- Finally, save the changes you've made to wp-config.php and exit the editor.
![](./2.%20Launch%20WordPress%20Instance/Configure%20WordPress%20to%20Connect%20to%20RDS/2.Open-wp-configphp-for-editing.png)
![](./2.%20Launch%20WordPress%20Instance/Configure%20WordPress%20to%20Connect%20to%20RDS/3.db-details.png)

## 14. Create an AMI from Your Instance
1. Go to the Instances page:
- In the EC2 Dashboard, click on Instances under "Instances."
2. Select the Instance:
- Locate the instance you want to create an image from.
- Select the checkbox next to the instance to highlight it.
3. Create Image:
- Right-click the selected instance, or click the "Actions" button at the top.
- From the dropdown menu, go to Image and templates > Create Image.
![](./2.%20Launch%20WordPress%20Instance/create-image/1.create.png)
4. Configure the Image Settings:
- In the Create Image dialog box, configure the following options:
- Image name: Provide a name for your AMI.
- Image description: Provide an optional description for the AMI.
![](./2.%20Launch%20WordPress%20Instance/create-image/2.name-and-description.png)
5. Click Create Image:
- After configuring the settings, click Create Image.
- AWS will start creating the AMI, and you'll be notified when it's completed.
![](./2.%20Launch%20WordPress%20Instance/create-image/3.create.png)

## 15. Setup Application Load Balancer (ALB)
**Objective:** Set up an Application Load Balancer to distribute incoming traffic among multiple instances, ensuring high availability and fault tolerance.

STEPS:
1. Security Group:
- Create Security group, ensure it allows HTTP/HTTPS traffic.
![](./6.%20Set%20Up%20the%20Application%20Load%20Balancer%20ALB/security-group/1.sg-config.png)
![](./6.%20Set%20Up%20the%20Application%20Load%20Balancer%20ALB/security-group/2.create.png)

2. Create Target Group:
- Choose target type: Instances
![](./6.%20Set%20Up%20the%20Application%20Load%20Balancer%20ALB/target-group/1.instances.png)

- Name: Enter a name (e.g., DigitalBoost-TG).
- Protocol: Choose HTTP and port 80.
- VPC: Select DigitalBoostVPC.
![](./6.%20Set%20Up%20the%20Application%20Load%20Balancer%20ALB/target-group/2.name.png)

- Health Check Settings: Set health check path to /.
Register Targets: Add the WordPress EC2 instances to the target group.

3. Configure Listener Rules:
- Listeners: Configure listener rules to forward traffic to the target group (DigitalBoost-TG).
![](./6.%20Set%20Up%20the%20Application%20Load%20Balancer%20ALB/target-group/4.create-target.png)
![](./6.%20Set%20Up%20the%20Application%20Load%20Balancer%20ALB/target-group/5.select-target-group.png)

4. Register Targets: Add the WordPress EC2 instances to the target group.
![](./6.%20Set%20Up%20the%20Application%20Load%20Balancer%20ALB/target-group/3.register-target.png)

### 5. Create an Application Load Balancer (ALB):
- Navigate: EC2 Dashboard > Load Balancers > Create Load Balancer.
- Load Balancer Type: Select Application Load Balancer.
![](./6.%20Set%20Up%20the%20Application%20Load%20Balancer%20ALB/1.choose-alb-click-create.png)

- Name: Enter a name (e.g., DigitalBoost-ALB).
- Scheme: Choose Internet-facing.
![](./6.%20Set%20Up%20the%20Application%20Load%20Balancer%20ALB/2.select-internet-facing.png)

- VPC: Select DigitalBoostVPC.
- Subnets: Select the PublicSubnet (both Availability Zones).
![](./6.%20Set%20Up%20the%20Application%20Load%20Balancer%20ALB/3.vpc-public-subnets.png)

- Security Groups: Create/select a security group that allows HTTP and HTTPS traffic.
- Listeners: Add listeners for HTTP (port 80) and HTTPS (port 443).
![](./6.%20Set%20Up%20the%20Application%20Load%20Balancer%20ALB/4.listeners-target-group-sg.png)

- Scroll down and click `Create load balancer`
![](./6.%20Set%20Up%20the%20Application%20Load%20Balancer%20ALB/5.click-create-load-balancer.png)

## 16. Auto Scaling Group Setup:
**Objective:** Implement Auto Scaling to automatically adjust the number of instances based on traffic load.

**Step 1:** **Create a Launch Template:** A launch template defines the instance configuration (AMI, instance type, key pair, etc.) that your Auto Scaling Group will use.
1. Navigate to the EC2 Console:
- Open the EC2 Dashboard in the AWS Management Console.
- Go to Launch Templates under Instances.
- Click Create launch template.
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/1.click-create.png)
- Provide a name (e.g., DigitalBoostTemplate).
- Provide template version description
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/2.name-version.png)

- Select the AMI created from your private instance.
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/3.select-myAMI.png)

- Choose the instance type (e.g., t3.micro or any type you prefer).
- Select your key pair for SSH access
- Select Private Subnet
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/4.instance-type-subnet.png)

- Select an existing security group
- Scroll down and click on Advanced Details to expand this section. 
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/5.select-security-group-click-advanced-detail.png)
- In the User Data field, you can add a script that will be executed when the instance starts. This is typically used to set up software, install packages.
- Click Create launch template
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/6.UserData-ALB-ASG-Script.png)

**Step 2: Create an Auto Scaling Group:** The Auto Scaling Group will manage the scaling and health of your instances.
- In the AWS Console, search for Auto Scaling Groups under the EC2 Dashboard.
- Click on Create Auto Scaling group.
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/Create%20an%20Auto%20Scaling%20Group/1.cllick-create-asg.png)

- Provide a name (e.g., DigitalBoostASG).
- Select the launch template you created earlier.
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/Create%20an%20Auto%20Scaling%20Group/2.name-select-launch-template.png)

- Click Next
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/Create%20an%20Auto%20Scaling%20Group/3.click-next.png)

- Select the VPC and the private subnets where the instances will launch.
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/Create%20an%20Auto%20Scaling%20Group/4.vpc-subnet-next.png)

- Under Attach to a load balancer, select Attach to an existing load balancer.
- Choose your existing Application Load Balancer (ALB) or Classic Load Balancer (CLB) from the dropdown list.
- Target Group: Select the appropriate target group associated with your Load Balancer.
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/Create%20an%20Auto%20Scaling%20Group/5.attach-load-balancer.png)

- Click Next
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/Create%20an%20Auto%20Scaling%20Group/6.next.png)

- Set the desired number of instances (e.g., 2 instances).
- Set the minimum number of instances (e.g., 1 instance).
- Maximum Capacity: Set the maximum number of instances to scale (e.g., 3 instances).
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/Create%20an%20Auto%20Scaling%20Group/7.group-size.png)

- Target Tracking Scaling: Automatically scale based on a metric (e.g., CPU utilization).
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/Create%20an%20Auto%20Scaling%20Group/8.policy.png)

- Click Next
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/Create%20an%20Auto%20Scaling%20Group/6.next.png)

- Click Next
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/Create%20an%20Auto%20Scaling%20Group/11.next.png)

- Review and Create the Auto Scaling Group
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/Create%20an%20Auto%20Scaling%20Group/12.preview-create-auto-scaling-group.png)
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/Create%20an%20Auto%20Scaling%20Group/13.created.png)

- Verify that the desired number of instances is launched in the private subnets.
![](./7.%20Launch%20EC2%20Instances%20in%20Auto%20Scaling%20Group/Create%20an%20Auto%20Scaling%20Group/14.instances-running.png)

## Conclusion
The DigitalBoost WordPress Deployment on AWS was successfully set up, providing a scalable, secure, and highly available environment for hosting WordPress websites. Further enhancements, like RDS Multi-AZ deployment and Auto-Scaling optimization, can improve the resilience and performance of the infrastructure.