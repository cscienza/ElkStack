## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![alt text](https://github.com/cscienza/ElkStack/blob/main/Images/networkdiag.png "Network Diagram")

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.
  - https://github.com/cscienza/ElkStack/blob/main/Files/install-elk.yml
  - https://github.com/cscienza/ElkStack/blob/main/Files/filebeat-playbook.yml
This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant, in addition to restricting access to the network from unauthorized or unknown users. Placing our containers and jumpboxes behind a load balancer is critical to maintain availabilty of critical network systems in the event of an attack, as well as to obfuscate the locations of the critical systems running on our jumpbox such as DVWA, Filebeat, and Metricbeat. The purpose of the jumpbox is to provide a highly restriced access point or 'door' into our network systems. This alos allows for much easier configuration and administration of the systems over time, as everything is accessible.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system files.
Filebeat is used to collect, organize, and send data to Elasticsearch or Logstash by monitoring for log data and events.
Metricbeat is a metrics collections utility that will survey and record various metrics of the system/OS it is running on, and then make that data available to other utilities such as Logstash and Elasticsearch.

The configuration details of each machine may be found below.


| Name           | Function   | IP Address | Operating System   |
|----------------|------------|------------|--------------------|
| JumpBox-Offsec | Gateway    | 10.1.0.4   | Linux/ubuntu 18.04 |
| Web1-Offsec    | Webserver  | 10.1.0.5   | Linux/ubuntu 18.04 |
| Web2-Offsec    | Webserver  | 10.1.0.8   | Linux/ubuntu 18.04 |
| Web3-Offsec    | Webserver  | 10.1.0.7   | Linux/ubuntu 18.04 |
| Elk-Server     | Monitoring | 10.0.0.5   | Linux/ubuntu 18.04 

|### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 99.85.64.128

Machines within the network can only be accessed by JumpBox at adress 10.1.0.4.
- The ElkServer machine can only be reached by two methods. SSH from the Jumpbox at address 10.1.0.4 to allow configuration changes, and through a web interface from the public IP of my local machine at 99.85.64.128.

A summary of the access policies in place can be found in the table below.

| Name           | Publicly Accessible | Allowed IP Address    |
|----------------|---------------------|-----------------------|
| JumpBox-Offsec | Yes                 | 99.85.64.128          |
| Web1-Offsec    | No                  | 10.1.0.4              |
| Web2-Offsec    | No                  | 10.1.0.4              |
| Web3-Offsec    | No                  | 10.1.0.4              |
| Elk-Server     | Yes                 | 10.0.0.5/99.85.64.128 |
| Load Balancer  | Yes                 | 99.85.64.128          |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because this keep administrative time and costs much lower, as well as making the network for more responsive to updates and configuration changes, since any updates to configuration will immediately affect any or all machines. This greatly reduces potential downtime as even given a worse case scenario of a  catastrophic event on the newtork, the automated redeployment is trivial compared to manual redeployment.

The playbook implements the following tasks:
Installs Docker.io
Installs pip3 and Python Docker Module
Increases the Virtual Memory available to Docker Container
Downloads, installs, and runs an ELK container image, and configures ports 5044,5601, and 9200 for TCP access
Configures ELK to always run on system startup
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![alt text](https://github.com/cscienza/ElkStack/blob/main/Images/dockersuccess.jpg "Docker Success")

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.1.0.5, 10.1.0.7, 10.1.0.8
We have installed the following Beats on these machines:
- Filebeat, Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat allows the collection of system log files at predifined locations so allow for easier monitoring of network and system events.
- Metricbeat allows the collection of metric data from remote servers such hardware information, usage statics, and service/applications running on the monitored machine.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install-elk.yml file to the /etc/ansible directory.
- Update the /etc/ansible/hosts file to include to all operational webservers including the IP the ELK server will be hosted from in its own group headed by the title 'ELK'.
- Run the playbook, and navigate to http://52.250.115.91:5601/app/kibana to check that the installation worked as expected.
