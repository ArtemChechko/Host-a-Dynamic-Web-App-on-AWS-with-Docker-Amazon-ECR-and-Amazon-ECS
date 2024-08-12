---

# Hosting a Dynamic Web App on AWS with Docker, Amazon ECR, and Amazon ECS

This project outlines the steps to host a dynamic web application on AWS using Docker, Amazon ECR, and Amazon ECS. Below is a comprehensive guide to set up the infrastructure and deploy your application.

## Prerequisites

1. **Sign Up for a GitHub Account**: Create a [GitHub account](https://github.com/join) if you don't have one.
2. **Create SSH Key Pairs**:
   ```bash
   ssh-keygen -t rsa -b 2048
   ```
3. **Add Public SSH Key to GitHub**: Copy the contents of the generated public key file (e.g., `~/.ssh/id_rsa.pub`) to your GitHub SSH settings.
4. **Install Git**: Download and install Git on your computer. Verify the installation:
   ```bash
   git --version
   ```
   Configure Git with your name and email:
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "youremail@example.com"
   ```
5. **Install Visual Studio Code**: Download and install [Visual Studio Code](https://code.visualstudio.com/).
6. **Create a Docker Hub Account**: Sign up for a [Docker Hub account](https://hub.docker.com/).
7. **Enable Virtualization**: Ensure virtualization is enabled in your system's BIOS/UEFI settings.
8. **Install Docker**: Download and install Docker. Verify the installation:
   ```bash
   docker -v
   ```

## AWS Infrastructure Setup

9. **Build a Three-Tier AWS Network VPC**: Follow [this guide](https://stratus10.com/blog/aws-best-practices-3-tier-infrastructure) to build a three-tier VPC from scratch.
10. **Create NAT Gateways**: Set up NAT gateways with private route tables to enable outbound internet access for instances in private subnets.
11. **Set Up Security Groups**: Configure security groups to control inbound and outbound traffic for your AWS resources.
12. **Set Up a MySQL RDS Instance**: Launch a MySQL RDS instance within your VPC for database management.
13. **Register a Domain Name in Route 53**: Use Amazon Route 53 to register a new domain name for your application.

## Application Code Management

14. **Create a Repository for Application Code**: Create a new GitHub repository to store your application’s source code.
15. **Add Application Code to the Repository**: Push your application code to the GitHub repository.
16. **Create a Repository for Dockerfile**: Create another GitHub repository specifically for your Dockerfile and related scripts.
17. **Create a Personal Access Token**: Generate a personal access token in GitHub for secure operations.

## Docker Image Creation

18. **Create a Dockerfile**: Write a Dockerfile to containerize your dynamic web application.
19. **Create a .gitignore File**: Add a `.gitignore` file to exclude unnecessary files from your repository.
20. **Add Build Arguments and Environment Variables**: Define build arguments and environment variables in the Dockerfile as needed.
21. **Create a Script to Build the Docker Image**: Write a shell script (`build_image.sh`) to automate the Docker image build process.
22. **Make the Script Executable**:
   - On Windows PowerShell:
     ```bash
     Set-ExecutionPolicy -ExecutionPolicy Unrestricted
     ```
   - On Linux/Mac:
     ```bash
     chmod +x build_image.sh
     ```
23. **Build the Docker Image**: Execute the shell script to build the Docker image for your application.

## AWS CLI and ECR Setup

24. **Install AWS CLI**: Download and install the [AWS CLI](https://aws.amazon.com/cli/).
25. **Create an IAM User**: Create an IAM user in AWS with programmatic access.
26. **Run AWS Configure**: Set up AWS CLI with your credentials:
   ```bash
   aws configure
   ```
27. **Create an Amazon ECR Repository**:
   ```bash
   aws ecr create-repository --repository-name <repository-name> --region <region>
   ```
28. **Push Docker Image to Amazon ECR**:
   - Retag the Docker image:
     ```bash
     docker tag <image-tag> <repository-uri>
     ```
   - Log in to ECR:
     ```bash
     aws ecr get-login-password | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
     ```
   - Push the Docker image:
     ```bash
     docker push <repository-uri>
     ```

## Database Migration

29. **Create a Key Pair in AWS**: Generate a key pair in the AWS Management Console for SSH access.
30. **Set Up a Bastion Host with EC2**: Launch an EC2 instance to act as a bastion host for secure database migration.
31. **Install Flyway on Your Computer**: Download and install [Flyway](https://flywaydb.org/) on your computer.
32. **Update the Flyway Configuration File**: Configure Flyway with your MySQL RDS instance credentials.
33. **Organize SQL Scripts in Flyway**: Place your SQL migration scripts in Flyway's SQL directory.
34. **Securely Run Flyway Migrate with an SSH Tunnel**:
   - Create an SSH tunnel:
     ```bash
     ssh -i <key_pair.pem> ec2-user@<public-ip> -L 3306:<rds-endpoint>:3306 -N
     ```

## Load Balancer and Security Setup

35. **Create an Application Load Balancer (ALB)**:
   - Create a target group with target type as IP addresses.
   - Set up the ALB with appropriate subnets and security groups.
36. **Register for an SSL Certificate in ACM**: Obtain an SSL certificate using Amazon Certificate Manager.
37. **Create an HTTPS Listener for the ALB**:
   - Add a listener for HTTPS traffic.
   - Configure the listener to forward traffic to the target group.
   - Modify the HTTP listener to redirect HTTP traffic to HTTPS.

## ECS Deployment

38. **Add Environment Variables to a .env File**: Store necessary environment variables in a `.env` file.
39. **Create an S3 Bucket for Environment File**: Create an S3 bucket and upload the `.env` file to it.
40. **Create an IAM Role for ECS Task Definition**: Create an IAM role with permissions for ECS tasks.
41. **Create an ECS Cluster**: Set up an ECS cluster to run your containerized application.
42. **Create a Task Definition**: Define an ECS task using your Docker image and environment variables.
43. **Create an ECS Service**: Deploy your application as a service within the ECS cluster.

## DNS Configuration

44. **Create a Record Set in Route 53**:
   - Set up an alias record in Route 53 that points to your ALB.

---

