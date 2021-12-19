# Cybersecurity
Project files for Linux SysAdmin Fundamentals, Azure private network for using by Red and Blue teams, etc.

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

  https://github.com/ValeriyPogosyan/Cybersecurity/blob/main/Diagrams/Networking.JPG

These files have been tested and used to generate a live ELK deployment on Azure. 
They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the install-elk.yml file may be used to install only certain pieces of it, such as Filebeat.

  https://github.com/ValeriyPogosyan/Cybersecurity/blob/main/Ansible/install-elk.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting downtime to the network.

      A load balancer defends an organization against distributed denial-of-service (DDoS) attacks.
	  It can do this this by shifting attack traffic from the corporate server to a public cloud provider.

 What is the advantage of a jump box?
 
 A jump server is a hardened and monitored device that spans two dissimilar security zones and provides a controlled means of access between them. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
	What does Filebeat watch for?_
	Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
	
    What does Metricbeat record?
	
	Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.
	Metricbeat helps you monitor your servers by collecting metrics from the system and services running on the server

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function 		 | IP Address | Operating System |
|----------|-----------------|------------|------------------|
| Jump Box | Gateway         | 10.1.0.5   | Linux            |
| Web-1    |dvwa image holder| 10.1.0.6   | Linux            |
| Web-2    |dvwa image holder| 10.1.0.7   | Linux            |
| ELKHost  |elk image holder | 10.0.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- 99.253.237.50

Machines within the network can only be accessed from container.

 Which machine did you allow to access your ELK VM? What was its IP address?
 
 dazzling_wright : 172.17.0.2

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | Any                  |
| Web-1    | No                  | 172.17.0.2           |
| Web-2    | No                  | 172.17.0.2           |
| ELKHost  | No                  | 172.17.0.2           |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
 
 What is the main advantage of automating configuration with Ansible?
 
 Ansible helps users to address complicated tasks to configure diverse settings of VMs and environments in automated fashion
 It also eliminates human errors since the process is using scripts. 

The playbook implements the following tasks:

	- Install python3-pip
	- Install Docker module
	- download and launch a docker elk container
	- Enable service docker on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/ValeriyPogosyan/Cybersecurity/blob/main/Diagrams/dockerps.JPG

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.1.0.6
- 10.1.0.7

We have installed the following Beats on these machines:

- filebeat
- metricbeat   


These Beats allow us to collect the following information from each machine:
	
 -  Filebeat are used to monitor the Elasticsearch log files, collect log events, and ship them to the monitoring cluster.
    The recent logs are visible on the Monitoring page in Kibana.

 -  Metricbeat is collecting data about Elasticsearch.
    It is the measurement of behaviour and usage of system resources that can be collected and monitored from the system.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

 Answer the following questions to fill in the blanks:
 
- _Which file is the playbook? Where do you copy it? 

   Playbooks are the files with Ansible code written in YAML format.
   Location: /etc/ansible

- _Which file do you update to make Ansible run the playbook on a specific machine? 
   How do I specify which machine to install the ELK server on versus which to install Filebeat on?
   
   filebeat-config.yml, metricbeat-config.yml
   
   ELK:
   
   FileBeat:
   
   
   
   
- Which URL do you navigate to in order to check that the ELK server is running?

	http://20.115.124.97:5601/app/kibana#/home
	
		
		

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._