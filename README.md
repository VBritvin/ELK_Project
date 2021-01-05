## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Network Diagram](https://github.com/VBritvin/ELK_Project/blob/main/Diagrams/Network_Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - [Installing ELK](https://github.com/VBritvin/ELK_Project/blob/main/Ansible/install-elk.yml) 
  - [Filebeat playbook](https://github.com/VBritvin/ELK_Project/blob/main/Ansible/filebeat-playbook.yml)
  - [Metricbeat playbook](https://github.com/VBritvin/ELK_Project/blob/main/Ansible/metricbeat-playbook.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly **resilient, reliable and responsive**, in addition to restricting **overload** to the network.
- **Load** **Balancing** plays an important security role as computing moves evermore to the cloud. The off-loading function of a **load balancer** defends an organization against distributed denial-of-service (DDoS) attacks. 
- A **jump box** is a secure computer that all admins first connect to before launching any administrative task or use as an origination point to connect to other servers or untrusted environments. **Jump boxes** are used to make the environment significantly more secure. **Jump boxes** are also great places for crossing security domains or forcing remote admins to VPN into before going on to further connect to a network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the **data** and system **logs**.
- **Filebeat** watches for the the log files or locations that are specified, collects log events, and forwards them either to Elasticsearch or Logstash for indexing. 
- **Metricbeat** collects the metrics and statistics and ships them to the output that is specified, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.

| Name                 | Function       | IP Address              | Operating System |
| -------------------- | -------------- | ----------------------- | ---------------- |
| Jump-Box-Provisioner | Gateway        | 10.0.0.10/52.188.105.22 | Linux            |
| Web-1                | Web Server     | 10.0.0.11               | Linux            |
| Web-2                | Web Server     | 10.0.0.12               | Linux            |
| ELK-VM               | ELK Server     | 10.1.0.4/104.42.255.32  | Linux            |
| RedTeam-LB           | Load Balancer  | 104.211.36.49           | Linux            |
| Workstation          | Access Control | External IP/Public IP   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the **ELK** machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- **Workstation Public IP through TCP 5601**

Machines within the network can only be accessed by **Workstation and Jump-Box-Provisioner**.
- **Workstation Public IP through TCP (p. 5601)**
- **Jump-Box-Provisioner through 10.0.0.10 through SSH (p. 22)**

A summary of the access policies in place can be found in the table below.

| Name                     | Publicly Accessible | Allowed IP Addresses                            |
| ------------------------ | :-----------------: | ----------------------------------------------- |
| **Jump-Box-Provisioner** |       **No**        | **10.0.0.10 through SSH (p. 22)**               |
| **Web-1**                |       **No**        | **10.0.0.11 through SSH (p. 22)**               |
| **Web-2**                |       **No**        | **10.0.0.12 through SSH (p. 22)**               |
| **ELK-VM**               |       **Yes**       | **Workstation Public IP through TCP (p. 5601)** |
| **RedTeam-LB**           |       **Yes**       | **Workstation Public IP through HTTP (p. 80)**  |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because **of Ansible that allows quickly and easily deploy multilevel apps. No need to write custom code for automating systems; we just list the tasks required to be done by writing a playbook, and Ansible will figure out how to get the systems to the state we need them to be.**

The playbook implements the following tasks:
- Specify a different group of machines as well as a different remote user:

  ` - name: Configure Elk VM with Docker
    hosts: elk
    remote_user: sysadmin
    become: true
    tasks:`

- Install the following services: 

`- name: Install python3-pip
  apt:
      force_apt_get: yes
      name: python3-pip
      state: present`

- Increase System Memory :

`- name: Use more memory
  sysctl:
        name: vm.max_map_count
        value: '262144'
        state: present
        reload: yes`

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![docker ps](https://github.com/VBritvin/ELK_Project/blob/main/Diagrams/Docker_ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- **Web-1 10.0.0.11**
- **Web-2 10.0.0.12**

We have installed the following Beats on these machines:
- **ELK-VM, Web-1, Web-2;**
- **The ELK Stack installed: Filebeat ,Metricbeat.**

These Beats allow us to collect the following information from each machine:
- **Filebeat: log events**
- **Metricbeat: metrics and system statistics**

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- **Copy the https://github.com/VBritvin/ELK_Project/blob/main/Ansible/metricbeat-playbook.yml and https://github.com/VBritvin/ELK_Project/blob/main/Ansible/filebeat-playbook.yml to /etc/ansible/roles.**
- **Update the /etc/ansible/hosts file to include the IP addresses of ELK server and Web servers.**
- **Run the playbook, and navigate to HTTP 5601 to check that the installation worked as expected.**

- **Playbooks are the files where Ansible code is written. Playbooks are written in YAML format. YAML stands for Yet Another Markup Language. Playbooks are one of the core features of Ansible and tell Ansible what to execute.** **All hosts are stored in a local Ansible inventory file called /etc/ansible/hosts.**

- **To make Ansible run the playbook on a specific machine we need to update `/etc/ansible/hosts` by adding specific IP addresses.** 

- How do I specify which machine to install the ELK server on versus which to install Filebeat on? - 

  **For ELK server:**

  - **Copy the https://github.com/VBritvin/ELK_Project/blob/main/Ansible/install-elk.yml**
  - **Run the playbook using this command : `ansible-playbook install-elk.yml`**

  **For Filebeat :**

  https://github.com/VBritvin/ELK_Project/blob/main/Ansible/filebeat-playbook.yml

  ![Filebeat](https://github.com/VBritvin/ELK_Project/blob/main/Diagrams/Filebeat.png)

  **For Metricbeat:**

  https://github.com/VBritvin/ELK_Project/blob/main/Ansible/metricbeat-playbook.yml

![Metricbeat](https://github.com/VBritvin/ELK_Project/blob/main/Diagrams/Metricbeat.png)

- _Which URL do you navigate to in order to check that the ELK server is running? - 
- ![Kibana](https://github.com/VBritvin/ELK_Project/blob/main/Diagrams/Kibana.png)

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

