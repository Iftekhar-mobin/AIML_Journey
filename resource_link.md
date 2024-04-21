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
