Cal.com on Azure - Deployment Guide

Project by Riya Vijay Shukla

📄 Description

This project demonstrates how to deploy the open-source Cal.com scheduling platform on Microsoft Azure using Docker Compose. It integrates services like Virtual Machines (Linux), DNS Zone, Virtual Machine Scale Sets (VMSS), and monitoring alerts. Caddy is used as a reverse proxy with HTTPS enabled.

⚙️ Technologies Used

Microsoft Azure

Linux Virtual Machines (Ubuntu 22.04 LTS)

Docker & Docker Compose

PostgreSQL

Cal.com (open-source scheduling)

Caddy Server (for HTTPS and reverse proxy)

Azure DNS Zone

Azure Virtual Machine Scale Sets (VMSS)

Azure Monitor & Alerts

✅ Step-by-Step Deployment Instructions

2.1 Step 1: Create a Linux VM on Azure

Go to Azure Portal

Navigate to Virtual Machines > Create

Choose:

Resource group: calcom-project

VM name: calcom-linux-vm

Image: Ubuntu 22.04 LTS

Size: B1s or B1ms (for demo)

Authentication: Password or SSH

Ports: Allow 22 (SSH), 80 (HTTP), and 443 (HTTPS)

Click Review + Create and wait for deployment to complete

2.2 Step 2: Connect to VM and Clone Docker Repo

SSH into the VM:

ssh azureuser@<your-vm-ip>
sudo apt update && sudo apt install git -y

Create a folder and clone repo:

mkdir calcom-docker
cd calcom-docker
git clone https://github.com/calcom/docker.git

2.3 Step 3: Setup Environment Variables

Copy and edit the .env file:

cp .env.example .env
vi .env

Generate a secure secret:

openssl rand -hex 32

2.4 Step 4: Docker Compose Setup

Install Docker & Docker Compose:

sudo apt install docker.io docker-compose -y
sudo systemctl enable docker
sudo systemctl start docker

Verify installation:

docker --version
docker-compose --version

Edit docker-compose.yaml if needed

2.5 Step 5: Start the Cal.com App

cd docker
docker-compose up -d
docker ps

Expected containers:

calcom-web

calcom-db

calcom-caddy

3. Networking & Domain Setup

3.1 Step 6: Configure DNS (Azure DNS Zone)

Go to Azure > DNS Zones

Create a DNS Zone (e.g., calcom.local)

Add an A record:

Name: @

Type: A

IP Address: Your VM's public IP

3.2 Step 7: Configure HTTPS with Caddy

Create Caddyfile in the docker directory:

nano Caddyfile

Add:

calcom.centralindia.cloudapp.azure.com {
  reverse_proxy web:3000
}

Ensure ports 80 & 443 are allowed in the VM's networking rules

4. Cal.com Application Features

Booking and scheduling UI

Admin dashboard

Event creation and editing

Public event pages

Email confirmations

5. Scaling & Monitoring

5.1 Step 8: Set up Azure VM Scale Set (VMSS)

Go to Azure > Virtual Machine Scale Sets > Create

Use same region as main VM

Configure disk, networking, and enable autoscaling:

CPU threshold: > 70% → add instance

Min: 1, Max: 5 instances

5.2 Step 9: Creating Alerts for Failures

Use Azure Monitor > Alerts to set up custom rules

Example:

CPU > 80% for 5 mins → Notify or scale out

5.3 Step 10: Monitoring (Logs & Cost Alerts)

Enable diagnostics, logs, and usage alerts in Azure Monitor

Set up cost budget alerts to stay within resource limits

6. Project Submission

6.1 Step 11: Push Project to GitHub

git init
git remote add origin https://github.com/riyashukl/calcom-azure-project.git
git add .
git commit -m "Initial commit with Cal.com setup"
git push -u origin main

6.2 Step 12: Create ZIP File for Submission

cd ~
zip -r calcom-azure-project.zip calcom-azure-project
mv calcom-azure-project.zip /home/azureuser/
scp azureuser@<vm-ip>:/home/azureuser/calcom-azure-project.zip .

6.3 Step 13: Record and Submit Demo Video

Include:

VM setup & Docker containers

Access via HTTPS

GitHub and DNS Zone

Tools: OBS Studio, Xbox Game Bar, Zoom

Upload to YouTube or Drive

📽️ Demo Video Link: [Insert your video URL here]

7. References & Useful Links

Azure Portal

Cal.com GitHub

Docker Documentation

Caddy Server

