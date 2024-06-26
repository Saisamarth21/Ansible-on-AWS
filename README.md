# Ansible with AWS Setup 
This project details the setup of Ansible on AWS EC2 instances for efficient system management.

### About Ansible and AWS
**Ansible** : Ansible is an open-source automation tool that simplifies configuration management, application deployment, and task automation.

**AWS** : Amazon Web Services (AWS) offers a comprehensive suite of cloud computing services, providing scalable and flexible solutions for businesses and developers.

# Steps to Setup

## 1. Create AWS EC2 Instances
- Sign up for the [AWS Free Tier](https://aws.amazon.com/resources/create-account/) and navigate to the EC2 service.
- Create three Ubuntu instances with specifications available in the Free Tier.
- Ensure to create instances with key pairs and download the .pem key file to your local machine. Designate one instance as the master server and the others as servers.

## 2. SSH into AWS Instances

- Download an SSH client like [Termius](https://termius.com/download/windows) for SSH access.
- Connect to each instance using the public IP provided by AWS, the downloaded .pem key file, and PC name according to AWS instance, Usually its `ec2-user` or `ubuntu`.

## 3. Setup Ansible on Master Server

### i) Install Ansible

After connecting the instances to termius, use these commands to install Ansible in Master server :

```bash
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
sudo apt install ansible
```
### ii)Creating Private Key in Master server, then Making Private Key Secure
Copy the .pem key file downloaded locally to the master server, Then use this command to make the Private key secure :

```bash
chmod 400 key.pem
```
### iii) Setting Up the Inventory File

Edit the `/etc/ansible/hosts` file on your Ansible control node to define your inventory.

```bash
[servers]
Server-1 ansible_host=<ip address> ansible_connection=ssh ansible_user=ubuntu
Server-2 ansible_host=<ip address> ansible_connection=ssh ansible_user=ubuntu

[all:vars]
anisble_ssh_private_key_file=/home/ubuntu/key.pem
ansible_python_interpreter=/usr/bin/python3
```


### iv) Add Private Key to SSH Agent

To add private key to SSH Agent execute this:

```bash
ssh-agent bash
ssh-add ~/.ssh/key.pem
```
### v) Ping Other Servers

Verify connectivity to other servers from the master server:

```bash
ansible all -m ping -i /etc/ansible/hosts
```

## 4. Create and Run Ansible Playbooks

- Create playbooks using YAML files for desired tasks, some example playbooks are uploaded above.
- To run a playbook, use the following command:

```bash
ansible-playbook -i /etc/ansible/hosts file.yml
```
#### i)Executing the above example

- Create a playbook using YAML file and add this code `Ansible-on-AWS/install_nginx.yml`
- Execute the following command to run the playbook:

```bash
ansible-playbook -i /etc/ansible/hosts install_nginx.yml
```
- To print the Nginx version on the CLI for all servers:

```bash
ansible servers -i /etc/ansible/hosts -m command -a "nginx -v"
```
