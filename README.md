### Project Title: Building a Basic Cybersecurity Home Lab

#### 1. Project Overview
   - Objective: To create a virtual cybersecurity lab environment for learning and practicing security tools and techniques.
   - Tools Used: VMware, Kali Linux, Wazuh, Ubuntu, Nmap.

#### 2. Requirements
   - Hardware: A computer with a minimum of 16GB RAM and sufficient disk space (at least 100GB).
   - Software: VMware Workstation or VMware Player (for virtualization), ISO images for Kali Linux and Ubuntu, Wazuh installation files.

#### 3. Setup Steps

   Step 1: Setting Up VMware
   
1.1 Install VMware Workstation or VMware ESXi
Download and install VMware Workstation from VMware's official site (for desktops).
Alternatively, use VMware ESXi for a more advanced setup on a dedicated server.

1.2 Configure VMware Networking
Set up a Host-Only Network for internal communication between VMs.
Create a NAT Network if you need internet access for the VMs.
   

   Step 2: Create Virtual Machines
   
  = Install Kali Linux
   
= Download Kali Linux ISO from Kali Linux Downloads.

= Boot the VM from the ISO and install the system.

2.1 Ubuntu:
     
     Install Ubuntu Server 22.04
     
Download the Ubuntu Server ISO from Ubuntu Downloads.

Boot the VM from the ISO and follow the installation steps.

Update the system:

sudo apt update && sudo apt upgrade -y

2.2 Set Up SSH Access

Enable SSH on the server:

sudo apt install openssh-server -y

sudo systemctl enable ssh

sudo systemctl start ssh 


   Step 3: Install and Configure Wazuh for Monitoring
   
3.1 Overview of Wazuh

Wazuh is an open-source security monitoring tool. It will help you monitor and detect anomalies in your home lab.

3.2 Set Up Wazuh Server on Ubuntu Server

Install Docker on the Ubuntu Server:

sudo apt install docker.io -y

sudo systemctl enable docker

sudo systemctl start docker

Deploy Wazuh using Docker Compose:

Install Docker Compose:

sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

Create a directory for Wazuh:

mkdir ~/wazuh && cd ~/wazuh

Create a docker-compose.yml file:

yaml

version: '3.8'
services:
  wazuh:
    image: wazuh/wazuh
    container_name: wazuh
    ports:
      - "55000:55000"
      - "1514:1514/udp"
      - "5601:5601"
      
Start Wazuh:

docker-compose up -d

Access Wazuh Dashboard at http://<Ubuntu-Server-IP>:5601.

3.3 Install Wazuh Agent on both Ubuntu Server and Kali Linux:

sudo apt update && sudo apt install wazuh-agent -y

Configure the agent to connect to the Wazuh server:

sudo nano /var/ossec/etc/ossec.conf

Set the <server> tag with your Wazuh server IP.

Start the agent:

sudo systemctl enable wazuh-agent

sudo systemctl start wazuh-agent

#### 4. Testing and Usage

Network Scanning with Nmap

4.1 Basic Nmap Scans

Run a simple network scan to discover devices:

nmap -sP 192.168.0.0/24

Perform a port scan on the Ubuntu Server:

nmap -sV 192.168.0.10

4.2 Vulnerability Scanning with Nmap Scripts

Use Nmap's scripting engine to find vulnerabilities:

nmap --script vuln 192.168.0.10
     
4.3 Monitoring with Wazuh

Check Wazuh Dashboard for alerts and logs.

Review the logs on Ubuntu Server and Kali Linux to understand detected threats.

#### 5. Documentation and Reporting

Keep detailed notes on configurations, IP addresses, and any errors encountered.

Create a Markdown file or use a GitHub repository for documentation.

#### 6. Conclusion

Your basic home lab is now set up! You have a secure environment with a web server, a monitoring tool (Wazuh), and a testing platform (Kali Linux). Here are some suggestions for expanding your home lab:

Experiment with Vulnerability Assessment Tools like OpenVAS.

Integrate Metasploit on Kali Linux for advanced penetration testing.

Add Suricata or Snort for network intrusion detection.
