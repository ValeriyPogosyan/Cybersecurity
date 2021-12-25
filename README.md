# Cybersecurity
Project files for Linux SysAdmin Fundamentals, Azure private network for using by Red and Blue teams, etc.

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Project 1 Resource Overview](Diagrams/Networking.JPG)

These files have been tested and used to generate a live ELK deployment on Azure. 
They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the install-elk.yml file may be used to install only certain pieces of it, such as Filebeat.

  https://github.com/ValeriyPogosyan/Cybersecurity/blob/main/Ansible/install-elk.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting downtime to the network.

      A load balancer defends an organization against distributed denial-of-service (DDoS) attacks.
	  It can do this by shifting attack traffic from the corporate server to a public cloud provider.

 What is the advantage of a jump box?
 
 A jump server is a hardened and monitored device that spans two dissimilar security zones and provides a controlled means of access between them. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

	What does Filebeat watch for?
	
	Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
	
    What does Metricbeat record?
	
	Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.
	Metricbeat helps you monitor the servers by collecting metrics from the system and services running on the server.

The configuration details of each machine may be found below.


| Name     | Function 		 | IP Address | Operating System |
|----------|-----------------|------------|------------------|
| Jump Box | Gateway         | 10.1.0.5   | Linux            |
| Web-1    |dvwa image holder| 10.1.0.6   | Linux            |
| Web-2    |dvwa image holder| 10.1.0.7   | Linux            |
| ELKHost  |elk image holder | 10.0.0.4   | Linux            |

### Access Policies

The machines Web-1 and Web-2 on the internal network are exposed to the Internet only via loadbalancer's public IP.

The Jump Box machine can accept connections from the Internet.
Access to this machine is allowed from the following IP address:

- 99.253.237.50 which is the public IP address of my laptop.

 Which machine did you allow to access your ELK VM? What was its IP address?
 
 - from my personal computer's IP at port 5601
 - from the containers located at JumpBox via SSH azureuser@10.0.0.4
 
A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible   | Allowed IP Addresses                      |
|----------|-----------------------|-------------------------------------------|
| Jump Box | yes                   | Restricted to my personal computer IP only|
| Web-1    | yes, via loadbalancer | 40.78.145.13 at port 80                   |
| Web-2    | yes, via loadbalancer | 40.78.145.13 at port 80                   |
| ELKHost  | yes                   | Public IP at port 5601 (Kibana), SSH      | 

### Elk Configuration

 Ansible was used to automate configuration of the ELK machine. No configuration was performed manually.
 
 What is the main advantage of automating configuration with Ansible?
 
 Ansible helps users to address complicated tasks to configure diverse settings of VMs and environments in automated fashion
 It also eliminates human errors since the process is using scripts. 

The playbook implements the following tasks:

	- Install python3-pip
	- Install Docker module
	- download and launch a docker elk container
	- Enable service docker on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Screenshot docker ps](Images/dockerps.JPG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

- 10.1.0.6  (Web-1)
- 10.1.0.7  (Web-2) 

We have installed the following Beats on these machines:

- filebeat
- metricbeat   

These Beats allow us to collect the following information from each machine:
	
 -  Filebeat are used to monitor the Elasticsearch log files, collect log events, and ship them to the   monitoring cluster.
    The recent logs are visible on the Monitoring page in Kibana.
	
 -  Metricbeat is collecting data about Elasticsearch.
    It is the measurement of behaviour and usage of system resources that can be collected and monitored from the system.
	
	Filebeat data received:
	![Filebeat](Images/Capture%202.JPG)
	
	Metricbeat data received:
	![Metricbeat](Images/Capture%201.JPG)

	
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured.

Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- Copy the filebeat-config.yml file to /etc/ansible/filebeat-config.yml.
- Update hosts file to replace the IP address with the IP address of ELK machine:

		output.elasticsearch:
		hosts: ["10.1.0.4:9200"]
		username: "elastic"
		password: "changeme

		and
		
		setup.kibana:
        host: "10.1.0.4:5601"
		
- Create a playbook in /etc/ansible/roles/filebeat-playbook.yml

- Run the playbook, and navigate to http://[ElkHost Public IP]:5601/app/kibana#/home/tutorial/systemLogs 
  to check that the installation worked as expected.

 Answer the following questions to fill in the blanks:
 
- Which file is the playbook? Where do you copy it? 

   Playbooks are the files with Ansible code written in YAML format.
   Location: /etc/ansible

- Which file do you update to make Ansible run the playbook on a specific machine? 
                
				in /etc/ansible/hosts
				.....
				[webservers]
				#alpha.example.org
				#beta.example.org
				#192.168.1.100
				#192.168.1.110
				10.1.0.6 ansible_python_interpreter=/usr/bin/python3
				10.1.0.7 ansible_python_interpreter=/usr/bin/python3

				[elk]
				10.0.0.4 ansible_python_interpreter=/usr/bin/python3
				......

			Elk host is running in different virtual network so we have to add elk to a separate group.


   How do I specify which machine to install the ELK server on versus which to install Filebeat on?
   
   in filebeat-config.yml
   
   ELK:
   ....
   host: "10.0.0.4:5601" # TODO: Change this to the IP address of your ELK server
   ....
   
   FileBeat:
   ....
   hosts: ["10.0.0.4:9200"]
   ....
        
- Which URL do you navigate to in order to check that the ELK server is running?

	http://[ElkHost Public IP]:5601/app/kibana#/home
	
	Note: The IP address of ELK host is not static.
	
### Using Kibana

Observability

- Navigate to Kibana site : http://[ElkHost Public IP]:5601/app/kibana#/home 
  Note: after restarting the VMs the public IP should be updated
  
  Filebeat:
  
- Click Add Log Data => System Logs
- Under Getting Started select DEB then System logs dashboard
- We can see the Dashboards [Filebeat System] ECS panel for Web-1 and Web-2 with the follwing sections:

   -Syslog events by hostname [Filebeat System] ECS
   -Syslog hostnames and processes [Filebeat System] ECS
   -Syslog logs [Filebeat System] ECS
   
   The last section shows the follwing columns:
   

|  Time   |	 host.hostname   | process.name	  |   message  |
|---------|------------------|----------------|------------|



![Kibana Filebeat](Images/Kibana%20Filebeat.JPG)
										

    Metricbeat:
-Navigate to Kibana's home page
- Click Add metric data => Docker metrics
- Click metrics dashboard
- We can see the Dashboard [Metricbeat Docker] Overview ECS with follwing sections:
  
  -Docker containers [Metricbeat Docker] ECS
  -Number of Containers [Metricbeat Docker] ECS
  -CPU usage [Metricbeat Docker] ECS
  -Memory usage [Metricbeat Docker] ECS
  -Network IO [Metricbeat Docker] ECS
  
  Sample data for the first section:
  
|Name |	CPU usage (%)| 	DiskIO 	Mem (%)  | Mem RSS    |Number of Containers |
|-----|--------------|-------------------|------------|---------------------|
|dvwa |	34.2%	     |  81.301	5.4%	 |  127.7MB	  |    2                | 
|     | 34.2%	     |  81.301	5.4%	 |  127.7MB	  |    2                | 
  
    
   Adding various filters makes it possible to view exactly the data needed at the moment.

![Metric Filebeat](Images/Metric%20Filebeat.JPG)


_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

	....
	dpkg -i filebeat-7.4.0-amd64.deb
	filebeat modules enable system
	filebeat setup
	service filebeat start
	....



