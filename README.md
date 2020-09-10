## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/diagram_filename.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Playbook file may be used to install only certain pieces of it, such as Filebeat. 

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system metrics.

The configuration details of each machine may be found below.

| Name             | Function      | IP Address    | Operating System |
|------------------|---------------|---------------|------------------|
| Jump Box         | Gateway       | 10.0.0.1      | Linux            |
| Web1             | DVWA          | 10.0.0.5      | Linux            |
| Web2             | DVWA          | 10.0.0.6      | Linux            |
| ELK-VM           | ELK           | 10.0.1.4      | Linux            |
| Load Balancer    | Load Balancer | 52.186.159.63 | -                |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the load balancer and our ELK-VM can accept connections from the Internet. 
Access to the ELK-VM is only allowed from our local host IP via port 5601.

Machines within the network can only be accessed by our Ansible control machine in our docker container running on our Jump Box. 
The internal network IP of our Jump Box is 10.0.0.4. 

A summary of the access policies in place can be found in the table below.

| Name          | Internet Facing | Allowed IP Addresses  |   |   |
|---------------|-----------------|-----------------------|---|---|
| Jump Box      | Yes             | Client/LocalHost:22   |   |   |
| Web1          | No              | 10.0.0.4 via Docker   |   |   |
| Web2          | No              | 10.0.0.4 via Docker   |   |   |
| ELK-VM        | Yes             | Client/LocalHost:5601 |   |   |
| Load Balancer | Yes             | All                   |   |   |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows us to
spin up multiple machines at once.
There is no need to manually configure each individual machine - instead, all machines are uniformly configured.

The playbook implements the following tasks:
- Install Docker.io
- Install pip3 & Docker Python Module
- Downloads ELK image: sebp/elk:761
- Launch docker and run container running sebp/elk:761

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Images/docker_ps-root@ELK-Stack-VM_ ~.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web1 @ 10.0.0.5
- Web2 @ 10.0.0.6

We have installed the following Beats on these machines:
- FileBeat
- MetricBeat

These Beats allow us to collect the following information from each machine:
- FileBeat
	- Allows for collection of system log data from our DVWA machines
	- eg. any root login attempts will be logged, collected, and transmitted to our ELK stack via port 9200
Images/filebeat_example-2020-09-03 01_45_56-Discover - Kibana

- MetricBeat
	- Allows for collection of various server metrics such as system CPU, memory, processes, etc.
	- eg. total virtual memory of our machine
Images/metricbeat_example-2020-09-03 01_50_45-Discover - Kibana

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the ```<install-elk.yml>``` file to ```</etc/ansible>```.
- Update the ```install-elk.yml``` file to include filebeat and metricbeat
- Run the playbook, and navigate to ```<ELK machine IP Addr>:5601``` to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?
    - you copy it into 
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
