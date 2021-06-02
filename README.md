## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.  And later, additional files would add to this setup, and implement the ELK-Stack Server for monitoring the logs of the Web Servers shown below:

![image](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/images/VNet%20before%20ELK.png)

This was the starting point for the ELK-Stack Server Project.

In the Cloud, a Resource Group was defined (Red-Team), and within it, the Virtual Network (RedTeamNet).  A Network Security Group (or Firewall) was defined with rules to drastically reduce access to the network from the outside.  (This allowed only a single public IP SSH access to the network).  The Jump-Box was created first, with its Private IP.  The Jump-box also had a Public IP address so as to allow for connection from the outside.

Next the Web Servers (Web-1 first, and then Web-2 and Web-3) were created with only internal (private) IP addresses.

An SSH Key was generated and used between the outside (local workstation) and the Jump-Box.  Docker was downloaded using the curl command and installed on the Jump-Box.  Once the Docker container was established, it was started and attached to.  From within this container the deployment could commense.  Ansible was also downloaded using a curl command.

From within the Docker Container, another SSH key was generated and this was used to establish the needed security between the Jump-Box and each of the VMs (web servers).  A YAML script was written to "load" the individual VMs.  This was used to guarantee that each VM had the same setup.  Initially, the first VM (Web-1) was loaded and checked to be sure that everything from the script installed correctly.  The '[hosts](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/hosts)' file (located in the /etc/ansible/ folder in the ansible container) defined the destination by placing the private IP of the Web-1 server in the [webservers] section of the file.  Once this build was verified to be functioning, the other 2 VMs were created and their private IPs were added to the 'hosts' file and the Ansible Playbook ([my-playbook](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/my-playbook.yml)), was run again.

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the [configuration](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/filebeat-config.yml) file may be used to install only certain pieces of it, such as Filebeat.

  [filebeat-playbook](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/filebeat-playbook.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly _____, in addition to restricting _____ to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _____ and system _____.
- _TODO: What does Filebeat watch for?_
- _TODO: What does Metricbeat record?_

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| TODO     |          |            |                  |
| TODO     |          |            |                  |
| TODO     |          |            |                  |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the _____ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses_

Machines within the network can only be accessed by _____.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes/No              | 10.0.0.1 10.0.0.2    |
|          |                     |                      |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ...
- ...

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
