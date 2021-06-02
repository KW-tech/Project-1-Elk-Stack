## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.  And later, additional files would add to this setup, and implement the ELK-Stack Server for monitoring the logs of the Web Servers shown below:

![image](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/images/VNet%20before%20ELK.png)

This was the starting point for the ELK-Stack Server Project.

> In the Cloud, a Resource Group was defined (**Red-Team**).
>   Within this Resource Group, the Virtual Network (**RedTeamNet**) was defined.
>     A Network Security Group (or Firewall) (**RedTeamNSG**) was defined with rules to drastically reduce access to the network from the outside.  (This allowed only a single public IP SSH access to the network).
     
The **Jump-Box** was created first, with its Private IP.  The Jump-box also had a Public IP address, so as to allow for connection from the outside.

Next, the Web Servers (**Web-1** first, and then **Web-2** and **Web-3**) were created with only internal (private) IP addresses.

An SSH Key was generated and used between the outside (local workstation) and the Jump-Box.  This allowed for even more security.  The SSH Key was defined from my laptop at a specific Public IP address.  Even if someone else was in my network (they would have the same outgoing IP, but the Key would not be verified.

Docker was downloaded using the curl command and installed on the Jump-Box.  Once the Docker container was established, it was started and attached to.  From within this container the deployment could commense.  Ansible was also downloaded using a curl command.

From within the this Docker Container, another SSH key was generated and this was used to establish the needed security between the Jump-Box and each of the VMs (web servers).  A YAML script was written to "load" the individual VMs.  This was used to guarantee that each VM had the same setup.  Initially, only the first VM (Web-1) was loaded and checked to be sure that everything from the script installed correctly.  The '[hosts](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/hosts-a)' file (located in the /etc/ansible/ folder in the Docker container) defined the destination by placing the private IP of the Web-1 server in the [webservers] section of the file.  

**Note the IP info under the [webservers] label in this file**.

Once this build was verified to be functioning, the other 2 VMs were created and their private IPs were added to this updated '[hosts](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/hosts-b)' file (**note the [webservers] section in this file**) and then the Ansible Playbook ([my-playbook](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/my-playbook.yml)), was run again.

After this was verified to have run successfully, a Load Balancer was added.  Http Rules to it from the outside were added to the Network Security Group (firewall) and the 3 Web Servers were added to the Load Balancer's Back End Pool.  This gave the all 3 VMs the same Public IP address (that of the Load Balancer).

# The following begins the Elk Server Creation:

The following files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the [configuration](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/filebeat-config.yml) file may be used to install only certain pieces of it, such as Filebeat.

This configuration file had many commented out options.  

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

Load balancing ensures that the application will be highly available, in addition to restricting outside / unwanted access to the network.
  - Load Balancers guarantee a higher chance of applications being able to be reached when the server is called on from the internet.  Since the same applications are running on multiple servers, the Load Balancer does just what its name suggests.  It balances the load between all available servers.  If one goes down, the traffic will be directed to another.
  
  - The Jump Box, provides a crucial function.  It allows the loading of each server in a secure way that has very limited access.  This access is different from the applications running, that need accessed from the internet.  This loading allows for the quick spin-up and down of individual VMs.  One could be taken offline and another created and loaded with the same applications by running the playbook that created the first set of servers.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system metrics.
- Filebeat monitors the log files or locations that are specified and collects log events and forwards them for indexing (with Elasticsearch or LogStash)
- Metricbeat periodically collects metrics and statistics and ships them to the output specified, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  |  10.0.0.4  | Linux            |
| Web-1    | Server   |  10.0.0.6  | Linux            |
| Web-2    | Server   |  10.0.0.7  | Linux            |
| Web-3    | Server   |  10.0.0.8  | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Load Balancer machine can accept connections from the Internet.  It then directs the traffic to the individual Web Servers as the load dictates.  Access to this machine is only allowed from the following IP addresses: 24.149.74.212


Machines within the network can only be accessed by the Jump Box.
- The Jump Box can access the Elk Server (for loading / configuring)  The Local workstation (laptop) can access the Elk Server via a Web Interface (of Kibana) - (from the public IP address)  - its address is 24.149.74.212

A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible | Allowed IP Addresses |
|------------|---------------------|----------------------|
| Jump Box   | Yes (only fm laptop)| 20.185.88.203 (SSH)  |
| Web Servrs |  Only via Load Bal  | 168.62.51.90         |
|            |                     |                      |

After adding the rest of the network, My layout looked like this:

![image2](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/images/Final%20VNet.png)


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Since nothing manual was done, nothing will be missed in the eventuality that another server doing the same or similar montioring is needed.  Also if an update is needed, this can also be added to the YAML script and it can be rerun.  All needed info would be included in the install.

First the Elk Server Internal IP address is added to the '[hosts](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/hosts)' file with its own header [elk].  When the Ansible Playbook is run calling out this('elk') as the "hosts" variable - only those VMs listed will be affected by the playbook.

The [install-elk](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/install-elk.yml) playbook implements the following tasks:
- First, Docker.io is installed using apt 
- Next, Python3-pip is installed using apt
- Docker is then activated using pip
- The virtual memory is incerased to 262144 to allow for the processing of the log info.
- Finally the Elk container is downloaded and launched using the sebp image version 761 and the needed ports are opened
- systemd is utilized to enable the Docker on reboot of the VM.

After deploying this install, teh Elk-Server is SSH'd into and the command run:'sudo docker ps'.
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![image1](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/images/Elk%20-%20step%20%234%20.png)

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
