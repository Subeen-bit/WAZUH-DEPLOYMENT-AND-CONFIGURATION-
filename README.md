# WAZUH-DEPLOYMENT-AND-CONFIGURATION-

## Objective

This project focuses on implementing a centralized log collection and security monitoring solution using Wazuh in a Linux-based environment.
With the increasing need for cybersecurity monitoring, it is essential to collect, analyze, and correlate logs from different systems. In this project, Wazuh is used as a SIEM solution to collect logs from a monitored system (Kali Linux) and analyse them in a centralized server (Ubuntu).
The Wazuh agent installed on Kali Linux collects logs such as authentication logs, system logs, and security events, and forwards them to the Wazuh Manager installed on Ubuntu. The logs are then indexed and visualized in the Wazuh Dashboard for real-time monitoring and analysis.
The project demonstrates detection of security threats such as failed login attempts and SSH brute-force attacks. This setup enhances visibility into system activities and helps in identifying potential security threats effectively.

## Table of contents 

- INTRODUCTION
- PREREQUISITE 
- HARDWARE
- SOFTWARE 
- PROCEDURES       

## Introduction

Log collection in Linux is an important part of system monitoring and cybersecurity. Linux systems automatically generate logs that record system activities, user authentication attempts, and service operations. These logs are usually stored in the /var/log directory and help in understanding what is happening inside the system.
In this project, a centralized log monitoring system is implemented using Wazuh. The main objective is to collect logs from a Linux system and analyse them using a Security Information and Event Management (SIEM) solution to identify suspicious activities and potential security threats.
This setup provides better visibility, improves security monitoring, and helps in detecting attacks such as failed login attempts and unauthorized access in real time.

## Prerequisite

To successfully complete this project, basic knowledge of Linux is required, along with an understanding of networking fundamentals such as IP addresses and ports. A basic understanding of cybersecurity concepts is also important, as well as familiarity with setting up and using virtual machines.

## Hardware

The hardware requirements for this project include

- a system with at least an Intel i5 processor or an equivalent CPU
- a minimum of 4 GB RAM
- minimum 50 GB of storage to ensure smooth performance of virtual machines.

## Software

The following are the software’s used for this project. It is not necessary that the exact software’s are required to complete the objective. A Linux-based environment is used for monitoring and log collection. VirtualBox is used to create and manage virtual machines on a host system. Here, VirtualBox is used to create virtual Linux systems for both the monitoring server and the monitored machine.

- Ubuntu 22.04 LTS – Used as the monitoring system where the Wazuh manager is installed. It receives logs from the monitored system and performs analysis
- Kali Linux – Linux-based operating system which is used as the monitored system. It generates logs related to user activity, authentication attempts, and system processes.
- Wazuh (Version 4.7.5) – Wazuh is an open-source SIEM platform used for log collection, analysis, and security monitoring. It is installed on Ubuntu as the manager and on Kali Linux as the agent to collect and analyse logs in real time

## Procedures

# WAZUH MANAGER INSTALLATION (UBUNTU)

#### 1. Update System

`sudo apt update && sudo apt upgrade -y`

#### 2. Download Wazuh Installation Script

`curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh`

`ls`

#### 3. Run installation Script

Run the script with root privileges

`sudo bash wazuh-install.sh -a`

This installs:

- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard

And also shows the credentials to Login such as username and password

#### 4. Verify Installation

`sudo systemctl status wazuh-manager`

#### 5. Access Wazuh Dashboard

Open a browser and navigate to: `https://10.0.2.0`

Here the 10.0.2.0 is the IP address of Ubuntu(Wazuh Manager)

# WAZUH AGENT INSTALLATION (KALI LINUX)

#### 1. Add Wazuh Repository

`curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --dearmor -o /usr/share/keyrings/wazuh.gpg`

`echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt stable main" | sudo tee /etc/apt/sources.list.d/wazuh.list`

#### 2. Update Package List

`sudo apt update`

#### 3. Install Wazuh Agent 

`sudo apt install wazuh-agent=4.7.5-1 -y`

#### 4. Configure Manager IP

`sudo nano /var/ossec/etc/ossec.conf`

Update

Replace with your Ubuntu IP 

#### 5. Start Agent Service

`sudo systemctl enable wazuh-agent`

`sudo systemctl start wazuh-agent`

#### 6. Register Agent

`sudo /var/ossec/bin/agent-auth -m <IP of KALI LINUX>`

#### 7. Restart Agent

`sudo systemctl restart wazuh-agent`

# VERIFYING AGENT CONNECTION

On Manager (Ubuntu) check connected agents:

`sudo /var/ossec/bin/agent-control -l`




















