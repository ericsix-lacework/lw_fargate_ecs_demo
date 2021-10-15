# Terraform Lacework AWS Fargate ECS Cluster demo
  
### About
Terraform to deploy an AWS Fargate ECS Cluster and configure the Lacework Agent.  
  
The terraform also creates an ECS Fargate `Task Definition` to deploy the LW Agent as a sidecar using a volume map approach and sets up the IAM policy to deploy the main app from AWS ECR container registry.  
  
Currently the terraform runs on version `0.12.24` so you may wish to use (TF Switch)[https://github.com/warrensbox/terraform-switcher] to manage your different terraform versions easily !  
  
### Lacework Polygraph
Once you have your ECS Container deployed, the LW sidecasr will send all the details to the UI.  It will look like this.  
  
![Polygraph](/images/polygraph.png)
  
### How it looks in AWS
Once the terraform is complete, in AWS you will see your cluster in the ECS dashboard.  You will see it has a `Fargate` service and running task.  
  
![Cluster](/images/ecs_cluster.png)
  
There will be two containers:  
- The App container  
- The LW Datacollector Sidecar  
  
The App container will continue to run but the LW Sidecar will storp running after it finishes it config.  
  
![Container](/images/ecs_container.png)
  
### What you need to run the Terraform
You will need the following:  
 - AWS access  
 - A `pem` key in the AWS IAM  - (Docs here)[https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html]
 - Terraform installed  
 - AWS CLI  - (Docs here)[https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html]
 - A code editor like VS Code  
  
Here is a script to set up you laptop using `Brew` - (Code here)[https://github.com/anthonygrees/laptop_setup]
  
### Setup the Terraform
First you need the code !  
  
```bash
git clone https://github.com/anthonygrees/lw_fargate_ecs_demo

cd lw_fargate_ecs_demo
cd terraform
```
  
Next, create yourself a `terraform.tfvars` file with the following:  
 - node_counter: Is the number of demo VM's you need   
 - lw_token: Is the Lacework Agent Token  
  
Here is an example `tfvars` file:  
```bash
aws_profile = "default"
aws_region = "ap-southeast-2"
aws_key_pair_file = "~/.ssh/your_pem_file.pem"
aws_key_pair_name = "your_key_name"
lw_token = "99999999999999999999YOUR_TOKEN99999999999999999"
fargate_container_cpu = "256"
fargate_container_memory = "512"
app_image = "999999999999999.dkr.ecr.ap-southeast-2.amazonaws.com/your-nginx:latest"
```
  
With the Fargate CPU and Memory, make sure you stick to the correct sizes.  See the AWS docs - (CPU Task Guide)[https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-cpu-memory-error.html]  
  
### Initiate your Terraform
Execute the terraform. First run the initialise to ensure the plugins you need are installed:  
  
```bash
terraform init
```
  
Before you run Terraform to create your infrastructure, it's a good idea to see what resources it would create. It also helps you verify that Terraform can connect to your AWS account.  
  
```bash
terraform plan
```
  
### Run your Terraform
  
Now run the apply to create the infrastructure.  
  
```bash
terraform apply -auto-approve
```
  
### Apply Complete !
Once the run is complete you will get a list of the `server_ip` addresses.  
  