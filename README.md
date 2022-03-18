# Elk-Stack
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(https://github.com/Matthew-Brunken/Elk-Stack/blob/main/Diagrams/elk-stack-network.png?raw=true)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Ansible files may be used to install only certain pieces of it, such as Filebeat.

  - !(Ansible/roles/webserver-playbook.yml)
  - !(Ansible/roles/elk-playbook.yml)
  - !(Ansible/roles/filebeat-playbook.yml)
  - !(Ansible/roles/metricbeat-playbook.yml)
  - !(Ansible/files/filebeat-config.yml)
  - !(Ansible/files/metricbeat-config.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting traffic to the network.

Load balancers protect the availability aspect of security. They prevent requests from overwhelming servers in a server pool. They achieve this by portioning out incoming requests as equally as possible to the servers in a designated pool.

A jump box is a VM that is exposed to the internet and can access, typically through SSH, all other VMs on the local network. This provides administrators a central location to manage VMs, possibly using Ansible, and maintain security because not all VMs need to be public facing.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system resources.

The configuration details of each machine may be found below.

| Name     | Function                               | IP Address | Operating System |
|----------|----------------------------------------|------------|------------------|
| Jump Box | Gateway                                | 10.0.0.4   | Linux            |
| Web 1    | Application Server                     | 10.0.0.5   | Linux            |
| Web 2    | Application Server                     | 10.0.0.6   | Linux            |
| ELK      | Logging and Resource Monitoring Server | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 178.130.122.50

Machines within the network can only be accessed by the jump box.
- 10.0.0.4 (jump box)

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 178.130.122.50       |
| Web 1    | No                  | 10.0.0.4             |
| Web 2    | No                  | 10.0.0.4             |
| ELK      | No                  | 10.0.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because Ansible is faster and less error prone.

The playbook implements the following tasks:
- Install Docker to set up a container to house the ELK stack
- Increase virtual memory to accomodate the resource intensive ELK stack
- Download the image (sebp/elk:761) and start the container
- Enable docker container to start on VM boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

!(Diagrams/elk-docker-ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Webserver 1 (10.0.0.5)
- Webserver 2 (10.0.0.6)

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:

Filebeat consolidates the webservers' log information into a digest of important activities that’s easy to search through. This includes login attempts, sudo command usage, etc. Metricbeat monitors the health of the web servers by providing a readout of system usage (CPU, RAM, etc.)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the yml file to /etc/ansible directory.
- Update the hosts file to include the appropriate group [elk] with the corresponding local ip address.
- Run the playbook, and navigate to http://[your.VM.IP]:5601/app/kibana to check that the installation worked as expected.

### Commands to Install ELK stack
Start the VM with the Ansible control node

Start the Ansible control node container
`sudo docker start {container_name}`

Attach to the container
`sudo docker attach {container_name}`

Change directories to /etc/ansible
`cd /etc/ansible`

Update the ansible hosts file with the [elk] group with the VM's private IP address underneath using nano
`nano hosts`

Download ELK stack ansible playbook
`curl https://raw.githubusercontent.com/Matthew-Brunken/Elk-Stack/main/Ansible/roles/elk-playbook.yml > /etc/ansible/elk-playbook.yml`

Run the playbook
`ansible-playbook elk-playbook.yml`

If all steps in the playbook completed successfully, type the following IP address into the browser to access Kibana
http://[your.VM.IP]:5601/app/kibana
