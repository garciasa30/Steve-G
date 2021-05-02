## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://drive.google.com/file/d/1xANYz9ErC7IWq2gNZCaRxWl-cPPcgOwI/view?usp=sharing

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - ---
- name: install activity 1 modules
  hosts: webservers
  become: true
  tasks:

  - name: install dockerio
    apt:
      name: docker.io
      state: present

  - name: install python3
    apt:
     name: python3-pip
     state: present

  - name: install docker
    pip:
      name: docker
      state: present
  - name: install dvwa
    docker_container:
            name: dvwa
            image: cyberxsecurity/dvwa
            state: started
            published_ports: 80:80

  - name: enable docker service
    systemd:
       name: docker
       enabled: yes

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly reliable and available, in addition to restricting access to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?
Load balancers play a vital role in security of the network by shifting incoming traffic from attackers preventing a DDoS (denial of service attack) attack. A major advantage of a jump box in the cloud network provides a secure way of accessing different security zones, while providing a controlled aceess between the seperate servers. Using a firewall before the jump box limits hackers abilities to access the jump box. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the Jumpbox and system network.
- _TODO: What does Filebeat watch for?_
Filebeat monitors the data about the file system such as: log files, collects log events, and forwards them to Elasticsearch for review.
- _TODO: What does Metricbeat record?_
Metricbeat records the metrics of the machine such as: uptime, cpu upload, 

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address               | Operating System |
|----------|----------|--------------------------|------------------|
| Jump Box | Gateway  | 10.1.0.9 52.158.240.88   | Linux            |
| Web1     | Server   | 10.0.0.6                 | Linux            |
| Web2     | Server   | 10.1.0.7                 | Linux            |
| Web3     | Server   | 10.1.0.4                 | Linux            |
| Elk VM   | Server   | 10.0.0.0 52.167.210.217  | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 71.193.254.30
- _TODO: Add whitelisted IP addresses_71.193.254.30

Machines within the network can only be accessed by Jump Box.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_52.158.240.88

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Elk VM   | No                  | 10.0.0.0 10.0.0.0/16 |
| Web1     | No                  | 10.1.0.6             |
| Web2     | No                  | 10.1.0.7             |
| Web3     | No                  | 10.1.0.4             |
| Jump Box | Yes                 | 52.158.240.88        |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_
-The main advantage of automation configuration with Ansible is it is written in python and it is quick to learn, flexible, and agentless, i.e., you don’t need to install any other software or firewall ports on the client systems you want to automate. You also don’t have to set up a separate management structure.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- SSH into Jump-box provisioner <ssh azadmin@52.158.240.88>
- Start the ansible docker <sudo docker start competent_hoover>
- Change directory to </etc/ansible/roles> then <nano elk-playbook.yml> to edit, save and exit.
- run the elk-playbook <ansible-playbook elk-playbook.yml>

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output] 
-https://github.com/garciasa30/Steve-G

-root@1f08425a2967:~# ansible-playbook /etc/ansible/elk-playbook.yml

PLAY [Config Web VM with Docker] ***************************************************************

TASK [Gathering Facts] *************************************************************************
ok: [10.0.0.0]

TASK [docker.io] *******************************************************************************
[WARNING]: Updating cache and auto-installing missing dependency: python-apt

changed: [10.0.0.0]

TASK [Install pip3] *****************************************************************************
changed: [10.0.0.0]

TASK [Install Docker python module] ************************************************************
changed: [10.0.0.0]

TASK [download and launch a docker web container] **********************************************
changed: [10.0.0.0]

PLAY RECAP *************************************************************************************
10.0.0.0                   : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_
- 10.1.0.6, 10.1.0.7, 10.1.0.4, and 10.0.0.9

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_
-10.1.0.4 (Web3)

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
- Filebeat collects SSH login attempts, date, time, user name, and if the login was accepted or invalid.
- Metricbeats collects CPU usage, memory usage, Network IO usage, and other hardware usage of the system.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the elk-playbook.yml file to /etc/ansible/roles/elk-playbook.yml.
- Update the elk-playbook.yml file to include...
- Run the playbook, and navigate to http://52.167.210.217:5601/app/kibana to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_ The playbok is defined by file name <playbook.yml> and you copy the playbook to a container where Ansible is installed in order to deploy the playbook. 
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- You update docker by running <sudo apt update>. Overall the hosts file allows for grouping of machines so you can decide where to deploy your resources.  
- _Which URL do you navigate to in order to check that the ELK server is running? 
- http://52.167.210.217:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._