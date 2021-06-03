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

The following files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the [configuration](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/filebeat-config.yml) file may be used to install only certain pieces of it, such as Filebeat.  And the [configuration](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/metricbeat-config.yml) for the Metricbeat.

These configuration files had many commented out options. Many of those I cut out of the configuration depicted in this document.

The individual playbooks that were used to install and launch Filebeat and Metricbeat are below.  Both are installed on each Webserver VM.  This is known by the "hosts" being defined in each of these files as 'webservers' (and are defined in the hosts file).

  [filebeat-playbook.yml](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/filebeat-playbook.yml)   
  [metricbeat-playbook.yml](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/metricbeat-playbook.yml)


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting outside / unwanted access to the network.
  - Load Balancers guarantee a higher chance of applications being able to be reached when the server is called on from the internet.  Since the same applications are running on multiple servers, the Load Balancer does just what its name suggests.  It balances the load between all available servers.  If one goes down, the traffic will be directed to another.
  
  - The Jump Box, provides a crucial function.  It allows the loading of each server in a secure way that has very limited access.  This access is different from the applications running, that need accessed from the internet.  This loading allows for the quick spin-up and down of individual VMs.  One could be taken offline and another created and loaded with the same applications by running the playbook that created the first set of servers.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system metrics.
- Filebeat monitors the log files or locations that are specified and collects log events and forwards them for indexing (with Elasticsearch or LogStash)
- Metricbeat periodically collects metrics and statistics and ships them to the output specified, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.

| Name      |  Function   | IP Address |  Operating System  |
|-----------|-------------|------------|--------------------|
| Jump Box  |   Gateway   |  10.0.0.4  | Linux Ub 18.04-LTS |
|   Web-1   |   Server    |  10.0.0.6  | Linux Ub 18.04-LTS |
|   Web-2   |   Server    |  10.0.0.7  | Linux Ub 18.04-LTS |
|   Web-3   |   Server    |  10.0.0.8  | Linux Ub 18.04-LTS |
| Elk Server| Log Collect |  10.1.0.4  | Linux Ub 18.04-LTS |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Load Balancer machine can accept connections from the Internet.  It then directs the traffic to the individual Web Servers as the load dictates.  Access to this machine is only allowed from the following IP addresses: Workstaion Local Public IP.


Machines within the network can only be accessed by the Jump Box.
- The Jump Box can access the Elk Server (for loading / configuring)  The Local workstation (laptop) can access the Elk Server via a Web Interface (of Kibana) - (from the public IP address)  - its address is the Workstation Local Public IP.

A summary of the access policies in place can be found in the table below.

| Name        | Publicly Accessible | Allowed IP Addresses |
|-------------|---------------------|----------------------|
| Jump Box    | Yes (only fm laptop)|  20.185.88.203 (SSH) |
| Web Servers |  Only via Load Bal  |     168.62.51.90     |
| Elk Server  |  Yes (via laptop)   |   20.94.49.96  (TCP) |

After adding the rest of the network, My layout looked like this:

![image2](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/images/Final%20VNet.png)

### Due to the Limitations of the Azure trial environment:
  
>  Several things becamce necessary in the setup and this actually highlights the flexibility and uses of the Cloud.
>  
>  First - only 4 virtual CPUs can be used within any network.  So a new unrelated network needed to be created.
>    This is the **ProjectNet** - it has its own seperate subnet 10.1.0.0/16.  Normally, this would cause communication issues.
>    However, "Peering" was initiated between the two Networks.  This allowed them to communicate across each other's subnets.
>    There was a peering from the Red Network to the Elk Network and one from the Elk Network to the Red Network.
>    This allowed both the Jump-Box to load the info onto the Elk Server and allowed the Elk Server to collect the log info
>    from the Web-servers.  All of this was done without the need to add special rules to the Security Groups (firewalls).
>  Next - the ProjectNet was also placed in a different Region and availability zone (this is not depicted in the diagram.)  
    
    
### Elk Configuration

Ansible was used to automate configuration of the ELK Server. No configuration was performed manually, which is advantageous because...
- Since nothing manual was done, nothing will be missed in the eventuality that another server doing the same or similar monitoring is needed.  Also if an update is needed, this can also be added to the YAML script and it can be rerun.  All needed info would be included in the install.

First the Elk Server Internal IP address is added to the '[hosts](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/hosts)' file with its own header [elk].  When the Ansible Playbook is run calling out this('elk') as the "hosts" variable - only those VMs listed will be affected by the playbook.

As you can see - the **hosts** File can be continuously modified and reused as more VMs or options to the existing VMs are added or changed.  And by adding a whole other Host Category, a different set of VMs or options on some of the existing can be implemented.

The [install-elk](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/install-elk.yml) playbook implements the following tasks:
- First, Docker.io is installed using apt 
- Next, Python3-pip is installed using apt
- Docker is then activated using pip
- The virtual memory is incerased to 262144 to allow for the processing of the log info.
- Finally the Elk container is downloaded and launched using the sebp image version 761 and the needed ports are opened
- systemd is utilized to enable the Docker on reboot of the VM.

After deploying this install, the Elk-Server is SSH'd into and the command run:'sudo docker ps'.
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![image1](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/images/Elk%20-%20step%20%234%20.png)

### Target Machines & Beats
This ELK Server is configured to monitor the following machines:
- The Elk Server actually just opened ports 5601, 9200, and 5044.  This was done in the **[install-elk](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/files/install-elk.yml)** file.

The Elk Server is listening.  The actual connections are done through the individual Web Servers (Web-1 thru Web-3).  It is defined in the MetricBeat and Filebeat Configure files. The Elk Server's internal IP (10.1.0.4) and ports are defined in the **Elasticsearch and Kibana** sections of the two files

We have installed the following Beats on these machines:
- **Filebeat**
- **Metricbeat**

These Beats allow us to collect the following information from each machine:
- Metricbeat collects application info from Apache and / or HAProxy and this would gather information any web service being accessed.
- Filebeat collects log files such as system logs or error logs.  This can be used to monitor login attempts and this was used to verify that is was actually working.  After the system was up and running - I made multiple failed attempts to SSH into each of the individual Web servers.  First was only Web-1.  Then a bit later I unsuccessfully attempted to log into each of the other two.  A screen shot is below:
![image6](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/images/Kibana%20failed%20login.png)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the configure files (filebeat-config.yml or metricbeat-configure.yml) to the /etc/ansible/files folder.  This location is defined in each respective playbook.
- Update the hosts file (as explained above) to include the internal IP addresses of each Web Server so that the beats are installed and configured on each server.
- Run the playbook, and navigate to Elk Server's Public IP (specifying the port 5601) to check that the installation worked as expected.

After running the **Filebeat-playbook.yml**, the following user interface was seen:

![image4](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/images/Elk%20Day%202%20Fileb.png)

And after the **Metricbeat-playbook.yml**:

![image5](https://github.com/KW-tech/Project-1-Elk-Stack/blob/main/images/Elk%20Day%202%20metric.png)

In Summation of the info already discussed:
- The Playbook files are those that end in -playbook.yml.  And they are copied into the /etc/ansible/roles/ folder - and run from there.

- The 'hosts' file is the file that is modified to determine which machine the playbook is run on.  And this is done by adding the target machine's internal IP address under the Host category - either **[webservers]** or **[elk]** in this scenario.  This 'hosts' file is located in the /etc/ansible/ folder along with the ansible.cfg file which was basically used to define the remote admin user for each machine so that the installation could continue.

- Once the playbooks have run, navigate to the Elk Server's Public IP address which was 20.94.49.96:5601/app/kibana, which included the port and the application in order to check that the ELK server is running.
