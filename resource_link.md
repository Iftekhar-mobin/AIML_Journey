# AWS CI/CD Setup Guide

This guide provides step-by-step instructions on setting up Continuous Integration and Continuous Deployment (CI/CD) pipelines using AWS services. The resources are collected from [this YouTube playlist](https://www.youtube.com/playlist?list=PLZoTAELRMXVPS-dOaVbAux22vzqdgoGhG).

## AWS Setup for CI/CD (ECR)

### Create IAM User

1. **Create IAM User**: Visit the AWS Management Console and create a new IAM user. Provide access permissions for AmazonEC2ContainerRegistryFullAccess and AmazonEC2FullAccess.

2. **Access Keys**: Generate CLI access keys for the IAM user and download the CSV file containing the keys.

### Create ECR Repository and EC2 Instance

1. **Create ECR Repository**: Create a private ECR repository with a name (e.g., `ifte_24_cicd`).

2. **Launch EC2 Instance**: Launch an EC2 instance, select Ubuntu, and configure security groups to allow HTTP and HTTPS traffic.

### Connect to EC2 Instance and Install Docker

Connect to the EC2 instance using PuTTY (for Windows) or SSH (for other platforms) and install Docker:

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
```

### GitHub Actions Setup

1. **Create Self-hosted Runner**: Create a new self-hosted runner on GitHub and follow the instructions to set it up.

2. **Configure Repository Secrets**: Go to repository settings on GitHub, and add secrets for AWS credentials and variables needed for the CI/CD process.

3. **Run Workflow**: Push commits to the repository to trigger the CI/CD workflow. If needed, rerun the workflow from the Actions tab.

### Additional Notes

- Ensure proper permissions and access controls are set up for AWS services.
- Regularly monitor the CI/CD pipelines for any errors or failures.
- Refer to AWS documentation for more advanced configuration options and troubleshooting.



## Resources are collected from https://www.youtube.com/playlist?list=PLZoTAELRMXVPS-dOaVbAux22vzqdgoGhG
## very very Important  https://www.youtube.com/watch?v=Xniji2m85LY&list=PLZoTAELRMXVPS-dOaVbAux22vzqdgoGhG&index=12

# Tutorials in short

1. **Create IAM User**
   - Provide user access to the AWS Management Console - optional
   - Skip permission option
   - Attach policies directly: AmazonEC2ContainerRegistryFullAccess, AmazonEC2FullAccess
   - Create user

2. **Generate CLI Access Keys**
   - Go to the security credentials tab
   - Create command line access keys (CLI)
   - Download CSV file

3. **Create ECR Repository**
   - Keep it private
   - Name it (e.g., ifte_24_cicd)

4. **Launch EC2 Instance**
   - Provide webserver name
   - Select Ubuntu
   - Create keypair/use existing
   - Allow HTTP and HTTPS traffic from the internet (checkbox)
   - Check instance status (running)

5. **Connect to EC2 Instance**
   - If on Windows, convert .pem to .ppk using PuTTYgen
   - PuTTY: Hostname → Public DNS, User name: `ssh -i "ifte_24_CICD.pem" ubuntu@ec2-16-170-170-144.eu-north-1.compute.amazonaws.com`
   - Install Docker:
     ```bash
     curl -fsSL https://get.docker.com -o get-docker.sh
     sudo sh get-docker.sh
     sudo usermod -aG docker ubuntu
     newgrp docker
     ```

6. **GitHub Actions Setup**
   - Create a new self-hosted runner on GitHub
   - Choose Linux
   - Run all the commands line by line from the GitHub generated runner script
   - Enter the name of runner: `self-hosted`
   - Continue with default values until `./run.sh`
   - Go back to runner menu on GitHub (selfHosted – Idle state)
   - Trigger a push to EC2 by making any commit

7. **Configure Repository Secrets and Variables**
   - Go to settings → secrets and variables → actions
   - Create new repository secret with information from the CSV file:
     - AWS_ACCESS_KEY_ID
     - AWS_SECRET_ACCESS_KEY
     - AWS_REGION (Top Right corner beside the profile)
     - AWS_ECR_LOGIN_URI (from ECR details, use only up to .com part)
     - ECR_REPOSITORY_NAME (e.g., ifte_24_cicd Last part after .com/)

8. **Rerun Workflow**
   - If something goes wrong, rerun the workflow to reinitiate the process

9. **Open Running Port**
   - Go to EC2 instance → security → security groups
   - Change inbound rules to open running port (e.g., 5000) or other port specified in the app.py:
     - Custom TCP -> port 5000 -> 0.0.0.0/0

10. **Cleanup Docker Resources (if needed)**
    - Remove containers and images if consuming all space:
      ```bash
      docker rmi -vf $(docker ps -aq)
      docker rmi -f $(docker images -aq)
      docker system df
      ```
