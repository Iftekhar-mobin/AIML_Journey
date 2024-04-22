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

### Tutorials
AWS Setup for CICD (ECR)
Create a IAM User 
ifte_24_CICD  → Provide user access to the AWS Management Console - optional  - skip → permission option → Attach policies directly → search AmazonEC2Container → AmazonEC2ContainerRegistryFullAccess + AmazonEC2FullAccess
(Attach this two permission policy) —> create user 

Go to security credentials tab → Access keys → create command line access keys (CLI) → download csv file
Create ECR repository → keep it private → Name it (ifte_24_cicd)
Create an EC2 instance → launch Instance → provide webserver name 
Select ubuntu (current pointing to AWS)
Create keypair/use existing
Allow http+https traffic from internet (checkbox)
Instances → status (running)
Go to security credentials tab → Access keys → create command line access keys (CLI) → download csv file
Connect to the EC2 instances → if windows then convert .pem to ppk from puttygen (select all files to see *.pem)
PuTTy → hostname → Public DNS  and User name “ ssh -i "ifte_24_CICD.pem" ubuntu@ec2-16-170-170-144.eu-north-1.compute.amazonaws.com” hence → ubuntu
Install docker 
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
Github → actions → runner → new self hosted runner → linux → run all the commands line by line from github generated runner script
Enter the name of runner:--> self-hosted   
Rest of the options are [Enter] use default values, continue till ./run.sh
Go back to runner menu Github [selfHosted – Idle state] → any commit → push to EC2
Go to settings→ secrets and variables → actions
New Repository secret → Put information from the Excel file 
AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY,  AWS_REGION (Top Right corner beside the profile), AWS_ECR_LOGIN_URI (from ECR details, use only up to .com part) 
ECR_REPOSITORY_NAME → ifte_24_cicd Last part after .com/
Actions → rerun workflow  -> if something goes wrong for reinitiate the process
Go to EC2 instance → security → security groups → change inbound rules → open running port 5000 or other port specified in the app.py 
Custom TCP -> port 5000 -> 0.0.0.0/0

If need to rerun and consumes all spaces by docker then remove container and images
docker rmi -vf $(docker ps -aq) 
Docker rmi -f $(docker images -aq)
Docker system df 
