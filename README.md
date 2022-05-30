From: Ed Colon
## Automated elk Stack Deployment

The files in this repository were used to configure the network depicted below.
 

These files have been tested and used to generate a live elk deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.
 
This document contains the following details:
•	Description of the Topology
•	Access Policies
•	elk Configuration
o	Beats in Use
o	Machines Being Monitored
•	How to Use the Ansible Build

Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available____, in addition to restricting _un-authorized____ to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_
Load balancers will provide the protection and availability of the data to distribute the “work-load” to multiple servers. The load balancer can also protect against attacks that deliberately impact the performance of the servers or the database, such as Denial of Service (DDoS) attacks. Using your laptop to SSH to access the jumpbox, eliminates attacks, such as a brute force attack. This method includes using NSG (Network Security Group to whitelist my laptop’s IP address to access the jumpbox server. This will prevent other endpoints to access any of the virtual machines from the internet except for accessing my laptop accessing the jump box.
Integrating an elk server allows users to easily monitor the vulnerable VMs for changes to the server metrics and system logs. 
What does Filebeat watch for?_
What does Metricbeat record?_
Two “soft-appliances” are Filebeat and Metricbeat. Filebeat records system logs, such as login attempts while Metricbeat records metric data, such as CPU/Memory usage.
The configuration details of each machine may be found below. I used the Markdown Table Generator to create the following table.
Name	Function	IP Address	                Operating System
RedTeam-Jumpbox	Gateway	20.242.6.138 / 10.0.0.4	        Linux (ubuntu 20.04)
elk-VM	elk Server	52.147.54.104 / 10.1.0.4	Linux (ubuntu 20.04)
Web-1	DVWA Server	10.0.0.5	                Linux (ubuntu 20.04)
Web-2	DVWA Redundancy Server	10.0.0.6	        Linux (ubuntu 20.04)
Web-3	DVWA Redundancy Server	10.0.0.10	        Linux (ubuntu 20.04)

Access Policies
The machines on the internal network are not exposed to the public Internet.
Only the gateway machine can accept connections from the Internet. Access to this machine is only allowed from the following IP address: 20.242.6.138
Machines within the network can only be accessed by the RedTeam-Jumpbox gateway machine at 10.0.0.4.
A summary of the access policies in place can be found in the table below.

Name		Publicly Accessible				Allowed IP Addresses
RedTeam-Jumpbox		Yes					68.174.129.140
elk-VM			Yes					68.174.129.140
Web-1			No					10.0.0.4
Web-2			No					10.0.0.4
Web-3			No					10.0.0.4

elk Configuration
Ansible was used to automate the configuration of the elk machine. No configuration was performed manually, which is advantageous because it allows complete flexibility to make changes when needed, across multiple servers. This automates the configurations to deploy to multiple machines without any concerns about individual configurations failing.
The ansible playbook implements the following tasks:
•	Install docker
•	Install Python3-pip
•	Install the docker using pip3
•	Increase virtual memory
•	Download and launch the elk:761 docker web container and allowed the following published ports to be permitted access: 5601:5601 9200:9200 5044:5044
•	Enable the docker service
The following screenshot displays the result of running docker ps after successfully configuring the elk instance.
 
Target Machines & Beats
This elk server is configured to monitor the following machines:
We have installed the following Beats on these machines:
Name	IP Address
Web-1	10.0.0.5
Web-2	10.0.0.6
Web-3	10.0.0.10

Filebeat and Metricbeat
Beats allow us to collect the following information from each machine: In 1-2 sentences, explain what kind of data each beat collects
Filebeat forwards the log data from your Web-servers (VMs). It can monitor the log files that you can specify, such as login attempts. The collection of log events will be transferred to the elk server.
Metricbeat collects the performance of the machine (metrics and statistical data), such as CPU usage, memory usage to include inbound/outbound traffic will be sent as data to the elk server.

Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SSH into the control node and follow the steps below:
•	Copy the playbook YAML files, to the /etc/ansible/files folder using:
cp ./Resources/filebeat-config.yml /etc/ansible/files/filebeat-config.yml
cp ./Resources/filebeat-playbook.yml /etc/ansible/files/filebeat-playbook.yml
cp ./Resources/metricbeat-config.yml /etc/ansible/files/metricbeat-config.yml
cp ./Resources/metricbeat-playbook.yml /etc/ansible/files/metricbeat-playbook.yml
cp ./Resources/install-elk.yml /etc/ansible/files/install-elk.yml
•	Update the filebeat-config and metricbeat-config files each to point towards the elk server IP address, username and password. (10.1.0.4, elastic & changeme, respectively, for both config files)
•	Update the /etc/ansible/hosts file with two separate sections for the private server IP addresses of the elk server and the webservers on the internal network. (see above for IP addresses) configure two sections: [webservers] [elkservers]
•	Run the filebeat and metricbeat playbooks, and ssh into each webserver (10.0.0.5, 10.0.0.6, and 10.0.0.10, respectively) and run "curl localhost/setup.php" to verify installation worked.
enter: ssh azdmin@server-ip-address (replace "server-ip-address" with above IP addresses
You should get an HTML code response on-screen. Next, navigate to the webservers via the load balancer's public IP address (20.119.167.182/setup.php) to check that the installation worked as expected.
•	Run the install-elk playbook and navigate to the elk server's Kibana webpage using the elk server's public IP address at the following URL (http:// http://52.147.54.104:5601/app/kibana#/home) to check that the installation worked as expected. You should see the Kibana homepage.
If everything runs as specified above, congratulations on your success!


