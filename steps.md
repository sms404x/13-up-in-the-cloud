# Task 13 – Up in the Clouds  
## Path A – Own Cloud Account  

## Step-1: Cloud Environment Details  

Cloud Provider: Microsoft Azure  
Subscription: Azure subscription 1  
Resource Group: Test_group  
Virtual Machine Name: VM-T13  
Region: Central India  
Availability Option: Availability Zone  
Selected Zone: Zone 2  

### Virtual Machine Configuration  

Image: Ubuntu Server 24.04 LTS – Gen2  
VM Architecture: x64  
VM Size: Standard B2als v2 (2 vCPUs, 4 GiB Memory)  
Security Type: Trusted Launch Virtual Machine  
Secure Boot: Enabled  
vTPM: Enabled  
Integrity Monitoring: Disabled  
Hibernation: Disabled  

### Cost Justification  

- Selected a low-cost VM size suitable for lab testing  
- Ubuntu image does not incur additional licensing costs  
- Azure Spot was not enabled  

### Authentication Configuration  

Authentication Type: SSH Public Key  
Username: azureuser  
SSH Key Format: RSA  
Key Pair Name: VM-T13_key  

Public Inbound Ports Enabled at Creation:  
- SSH (22)  
- HTTP (80)  
- HTTPS (443)  

## Step-2: Virtual Machine Deployment  

### Steps Performed  

- Logged into Azure Portal using personal Azure account  
- Navigated to Virtual Machines → Create  
- Selected image: Ubuntu Server 24.04 LTS – Gen2  
- Selected VM size: Standard B2als v2  
- Configured SSH public key authentication  
- Enabled Trusted Launch security  
- Enabled Secure Boot and vTPM  
- Assigned a Public IP address  
- Created Resource Group: Test_group  
- Configured Network Security Group (NSG)  
- Allowed inbound ports: SSH, HTTP, HTTPS  
- Deployed the VM successfully  

VM Public IP:  

20.193.252.**

## Step-3: Network Security Configuration  

### Inbound NSG Rules Configured  

Protocol | Port | Purpose  
TCP | 22 | SSH Access  
TCP | 80 | HTTP Web Server  
TCP | 443 | HTTPS Access  
ICMP | N/A | Ping Testing  

These rules ensure secure administrative access and web hosting capability.  

## Step-4: Connectivity Validation  

### Step-4.1: Inbound Connectivity Test (Ping from Kali)  

From Kali Linux:  

ping 20.193.252.**

Initially, ICMP traffic was blocked.  
Modified NSG to allow ICMP.  
Ping test succeeded, confirming inbound connectivity.  

Evidence:  
screenshot-1-kali-ping-vm.png  

### Step-4.2: SSH Access Verification  

From Kali Linux:  

ssh -i VM-T13_key azureuser@20.193.252.**

Successfully logged into the VM.  

Verified network interfaces inside VM:  

ip a  

This confirmed proper IP assignment and network configuration.  

Evidence:  
screenshot-2-ssh-ip-a.png  

### Step-4.3: Outbound Connectivity Test  

Checked public IP on Kali:  

curl ifconfig.me  

From inside the VM:  

ping <MY_PUBLIC_IP>  

Initially, the ping request did not receive replies. After deploying a new VM instance and retesting, the ping was successful.  

This confirms that outbound connectivity from the Azure VM is functioning correctly. The earlier failure was likely due to a temporary configuration issue (NSG rule behavior, firewall settings, or local network restrictions).  

Evidence:  
screenshot-3-vm-ping-my-public-ip.png  

## Step-5: Web Server Deployment (Port 80 Hosting)  

### Step-5.1: Nginx Installation  

Inside the VM:  

sudo apt update  
sudo apt install nginx -y  

Started and enabled Nginx:  

sudo systemctl start nginx  
sudo systemctl enable nginx  

### Step-5.2: HTTP Access Configuration  

Inbound rule for TCP 80 was already enabled during VM creation.  

Created custom webpage at:  

var/www/html/index.html  

Restarted Nginx:  

sudo systemctl restart nginx  

Accessed from local browser:  

http://20.193.252.**/  

The webpage loaded successfully, confirming:  

- Inbound HTTP access  
- Web server functionality  
- Internet-based API integration  

Evidence:  
screenshot-4-browser-http-vm.png  

## Step-6: Resource Cleanup and Decommissioning  

After validation:  

- Stopped the VM from Azure Portal  
- Deleted the Virtual Machine  
- Confirmed deletion of:  
  - OS Disk  
  - Network Interface  
  - Public IP  

Verified that the VM no longer appears in Azure resources.  

This ensures no ongoing cloud resource consumption or charges.  

## Final Outcome  

- VM successfully deployed in Azure (Trusted Launch enabled)  
- Inbound SSH connectivity verified  
- Outbound connectivity validated  
- Web server hosted on Port 80  
- Webpage accessible from local browser  
- Resources properly destroyed after testing  

# Task Completed Successfully.
