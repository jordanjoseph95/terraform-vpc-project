# AWS VPC with Terraform

This project deploys a small AWS network using Terraform. It builds a VPC, two public subnets across separate availability zones, an internet gateway, a route table, a security group, and an EC2 instance running nginx.

## What it builds

- A VPC with a 10.0.0.0/16 CIDR block
- Two public subnets in eu-west-2a and eu-west-2b
- An internet gateway attached to the VPC
- A route table sending outbound traffic through the gateway
- A security group allowing SSH (port 22) and HTTP (port 80)
- An EC2 instance running Ubuntu 22.04, with nginx installed via user data

## Why these choices

Two subnets across two availability zones give the setup basic fault tolerance. If one zone goes down, the other still has a working subnet.

The security group only opens the two ports the project needs. SSH for management, HTTP for the web server. Nothing else is exposed.

The EC2 instance uses a data source to look up the latest Ubuntu AMI instead of hardcoding an AMI ID. AMI IDs change over time and differ by region, so this keeps the code current without manual updates.

## How to run this

1. Install Terraform and the AWS CLI
2. Run `aws configure` and add your credentials
3. Clone this repo and move into the folder
4. Run `terraform init`
5. Run `terraform plan` to see what will be created
6. Run `terraform apply` and type yes to confirm

Once it finishes, Terraform prints the instance's public IP. Paste `http://<that-ip>` into your browser (use http, not https) and you'll see the nginx welcome page.

## Notes

This project uses raw Terraform resources rather than prebuilt modules. The goal was to understand what each piece of AWS networking does, not just deploy one quickly.
