## Automated ELK Stack Deployment


![Image](https://github.com/rin-0x91/elk-deployment/blob/master/Images/elk-deployment-network-diagram.png)

The files in this repository were used to configure the network depicted above.
These files have been tested and used to generate a live ELK deployment on Azure.
They can be used to either recreate the entire deployment pictured above, or alternatively, select portions of the Playbook file may be used to install only certain pieces of it, such as Filebeat. 

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA.
Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system metrics.

The configuration details of each machine may be found below.

|me             | Function      | IP Address               | OS     |
|---------------|---------------|--------------------------|--------|
| Jump Box      | Gateway       | 23.96.14.165 / 10.0.0.1  | Ubuntu |
| Web1          | DVWA          | 10.0.0.5                 | Ubuntu |
| Web2          | DVWA          | 10.0.0.6                 | Ubuntu |
| ELK-VM        | ELK Stack     | 104.42.38.141 / 10.1.0.4 | Ubuntu |
| Load Balancer | Load Balancer | 52.186.159.63            | -      |      

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the load balancer can accept connections from the Internet.
Using our RedTeamNSG, our Jump Box only accepts SSH requests from our local host via port 22..
Using our ELKStackNSG, access to the ELK-VM is only allowed from our local host IP via port 5601.
Our ELK-VM is also accessible via SSH by our Ansible control node.

Machines within the network can only be accessed by our Ansible control node within our Jump Box machine.
The internal network IP of our Jump Box is 10.0.0.4. 

A summary of the access policies in place can be found in the table below.

| Name          | Internet Facing | Allowed IP Addresses     |
|---------------|-----------------|--------------------------|
| Jump Box      | Yes             | Client/LocalHost:22      |
| Web1          | No              | 10.0.0.4:22              |
| Web2          | No              | 10.0.0.4:22              |
| ELK-VM        | Yes             | Client/LocalHost:5601    |
| Load Balancer | Yes             | All                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows us to
spin up multiple machines at once.
There is no need to manually configure each individual machine - instead, all machines are uniformly configured.

The playbook implements the following tasks:
- install Docker.io
- install pip3 docker python module
- increase virtual memory using sysctl
- download ELK image: sebp/elk:761 and launch a docker container running the image

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Image](https://github.com/rin-0x91/elk-deployment/blob/master/Images/dockerps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web1@10.0.0.5
- Web2@10.0.0.6

We have installed the following Beats on these machines:
- FileBeat
- MetricBeat

These Beats allow us to collect the following information from each machine:
- FileBeat
	- Allows for collection of system log data from our DVWA machines
	- eg. any root login attempts will be logged, collected, and transmitted to our ELK stack via port 9200
![Image](https://github.com/rin-0x91/elk-deployment/blob/master/Images/filebeat_kibana.png)

- MetricBeat
	- Allows for collection of various server metrics such as system CPU, memory, processes, etc.
	- eg. total virtual memory of our machine
![Image](https://github.com/rin-0x91/elk-deployment/blob/master/Images/metricbeat_kibana.png)

### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 
SSH into the control node and follow the steps below:
- Copy the `install-elk.yml` file to `/etc/ansible/`.
- Update the `install-elk.yml` file to include filebeat and metricbeat
- Run the playbook, and navigate to `<ELK machine IP Addr>:5601` to check that the installation worked as expected.
