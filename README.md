# Cal.com Deployment on Azure

This project deploys Cal.com using Docker Compose on an Azure Linux VM.

## Services Used
- Azure VM (Ubuntu)
- DNS Zone
- VM Scale Set (VMSS)
- Caddy (HTTPS via Let's Encrypt)
- PostgreSQL
- Alerts (Monitoring)
- Autoscaling rules

## Usage
1. Configure `.env`
2. Run `docker-compose up -d`
3. Access via `https://your-domain.com`

## Credits
Project by [Your Name]
Cal.com Deployment on Azure

Description

This project demonstrates how to deploy the open-source Cal.com scheduling platform on Microsoft Azure using Docker Compose. It integrates services like Virtual Machines (Linux), DNS Zone, Virtual Machine Scale Sets (VMSS), and monitoring alerts. Caddy is used as a reverse proxy with HTTPS enabled.

Technologies Used

Microsoft Azure

Linux VM

VM Scale Set (VMSS)

DNS Zone

Monitoring Alerts

Docker & Docker Compose

PostgreSQL

Caddy Server (for SSL and reverse proxy)

Cal.com (Open-source scheduling platform)

Step-by-Step Deployment Instructions

1. Create Linux Virtual Machine (VM)

Go to Azure Portal > Virtual Machines > Create

Select Ubuntu 20.04 LTS

Choose Standard B2s (2 vCPUs, 4 GiB RAM) or higher

Set authentication method to SSH or password

Open ports 22 (SSH), 80, and 443

2. Connect to VM via SSH

ssh <your-username>@<your-vm-public-ip>

3. Install Docker and Docker Compose

sudo apt update && sudo apt install docker.io docker-compose -y

4. Clone the Project Repository

git clone https://github.com/your-username/calcom-azure-project.git
cd calcom-azure-project

5. Setup Environment Variables

cp .env.example .env
# Fill in values in .env (API keys, secrets, database credentials)

6. Create and Configure Caddyfile

Create a file named Caddyfile:

calcom.centralindia.cloudapp.azure.com {
  reverse_proxy calcom:3000
}

7. Launch Cal.com Using Docker Compose

docker-compose up -d

This runs PostgreSQL, Cal.com, and Caddy with HTTPS automatically configured.

8. Configure DNS Zone in Azure

Go to Azure Portal > DNS Zones > Create

Name your zone (e.g., cloudapp.azure.com)

Add an A record pointing your subdomain (e.g., calcom) to the VM’s public IP

9. Create Monitoring Alerts

Navigate to your VM or VMSS

Go to Monitoring > Alerts > Create Alert Rule

Example rules:

CPU > 70% for 10 minutes → Alert or Scale out

HTTP 5xx > 10 → Alert

10. Set Up VM Scale Set (VMSS)

Go to Azure Portal > Virtual Machine Scale Sets > Create

Use Ubuntu and size with 4+ vCPUs

In Networking, disable Public IP (load balancer is needed)

Enable Autoscaling:

Rule 1: CPU > 70% for 10 mins → Increase instance count

Rule 2: CPU < 30% for 10 mins → Decrease instance count

Usage

Access your site via https://calcom.centralindia.cloudapp.azure.com

Login via Google or Microsoft account

Create meeting types, share booking links, and manage your schedule

Screenshots to Include (for ZIP submission or GitHub)

VM Creation (Azure portal)

SSH into VM

Docker Compose up command

DNS Zone and A Record

Caddy HTTPS/SSL working

Cal.com UI loaded in browser

Alert Rule configuration

VMSS scaling rules setup

Troubleshooting

SSL not working? Check:

Caddy logs (docker logs caddy)

DNS propagation

Ports 80 and 443 are open

App not loading? Ensure:

Database container is running

All environment variables are set correctly

VMSS not scaling?

Check metrics and autoscale rules

Verify monitoring data and alert configuration

License

This project uses Cal.com under its open-source license. See LICENSE for terms.

Author

Riya Shukla (riyashukla0301@gmail.com)


