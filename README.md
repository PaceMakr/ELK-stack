# ELK-Stack
This is the repo for a project that deploys an ELK stack in conjunction with 3 DVWA servers, controlled by a control node serving as a jumpbox provisioner, running on virtual Azure machines.

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

images/DVWA_Elk_Toplogy_Diagram.pdf

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

 YAML_Playbook_Files/Playbook-Alpha.yml.rtf
 YAML_Playbook_Files/filebeat-playbook.yml.rtf
 YAML_Playbook_Files/install-elk.yml.rtf
 YAML_Playbook_Files/metricbeat-playbook.yml.rtf

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.
- 
Load balancing ensures that the application will be highly secure, in addition to restricting incoming traffic to the network.
- The Load Balancer allows you to distribute traffic accross your VM's in order to prevent Ddos attacks. One of the major advantages of a Jumpbox is that it allows a user to secure, monitor and even configure multipe VM's from a single node .

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the files and system logs.
- Filebeat watches data across your system to identify if and when changes have been made to the system.
- Metricbeat creates metrics from the collected data from within the logs that filebeat watches for and sends them to the output that you specify.

The configuration details of each machine may be found below:

| Name                 | Function | IP Address   | OS    |
|----------------------|----------|--------------|-------|
| Jump-Box-Provisioner | Gateway  | 40.117.34.10 | Linux |
| ELK-VM               | Security | 40.78.106.9  | Linux |
| Web-1                | Server   | 10.0.0.7     | Linux |
| RedLB                | LoadBal  | 20.85.220.69 | ----- |
| Web-2                | Server   | 10.0.0.8     | Linux |
| Web-3                | Server   | 10.0.0.9     | Linux |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- HOME_IP

Machines within the network can only be accessed by the Jumpbox.
- Private IP: 10.0.0.4 ; Public IP: 40.117.34.10

A summary of the access policies in place can be found in the table below:

| Name                 | Publicly Accessible | Allow IP Address |
|----------------------|---------------------|------------------|
| Jump-Box-Provisioner | Yes                 | HOME_IP          |
| ELK-VM               | No                  | 10.0.0.4         |
| Web-1                | No                  | 10.0.0.4         |
| RedLB                | No                  | 10.0.0.4         |
| Web-2                | No                  | 10.0.0.4         |
| Web-3                | No                  | 10.0.0.4         |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- this allows you to apply the same configuration setup to multiple machines simultaneously.

The playbook implements the following tasks:
- Increases the Virtual Memory that the new VM can use
- Install docker.io
- Install python3, docker python module, and docker using pip
- Download and launch docker using an existing ELK image
- Enable the service

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

images/docker_ps_output.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1: 10.0.0.7 , Web-2: 10.0.0.8 , Web-3: 10.0.0.9

We have installed the following Beats on these machines:
- Filebeat: Installed
- Metricbeat: Installed

These Beats allow us to collect the following information from each machine:
- Filebeat collects logs from across your system.A good example of the type of data that filebeat collects is syslog which monitors a user's network devices and systems, then sends notifications if there is an issue found. 
- Metricbeat collects metrics from all systems and services like your CPU and it's usage data or storage, etc. then reprsents them visually.


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the beat config file to the ELK-vm. {/etc/ansible/files/filebeat-config.yml to etc/filebeat/filebeat.yml}
- Update the hosts file to include both the webserver & the Elk-vm's IP addresses. [nano /etc/ansible/hosts]
- Run the playbook [ansible-playbook /etc/ansible/roles/filebeat-playbook.yml], and navigate to the Kibana app to check that the installation worked as expected. [(in browser): "http://{$ELK_pub_IP}:5601/app/kibana"]


