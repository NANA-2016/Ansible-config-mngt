
# Ansible-config-mgt

## Ansible  project configuration



### inventory
 Files:dev,staging,uat,prod all .yml
 
 ### Playbooks
 File:Common.yml
 
![image](https://github.com/NANA-2016/Ansible-config-mgt/assets/141503408/29dcfe09-f055-48be-aaff-eb3c98f7c24c)

## Install jenkins

 Install jenkins and ensure it active and running with the commands attached below.

 sudo apt update
sudo apt install fontconfig openjdk-17-jre

sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo dnf upgrade

# Add required dependencies for the jenkins package

sudo dnf install fontconfig java-17-openjdk
sudo dnf install jenkins
sudo systemctl daemon-reload
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins

![image](https://github.com/NANA-2016/Ansible-config-mgt/assets/141503408/b0e37d83-2b64-4f1a-972b-45c72aef68c4)

use the command shown to clone jerkins ansible  to the git hub repository as the jump server  clone <ansible-config-mgt repo link>

## confuguring jenkins .

Attached screen shots show it is up and running with playbooks and inventory folder with there content in Ansible .

![image](https://github.com/NANA-2016/Ansible-config-mgt/assets/141503408/8f958eae-bb6e-4449-979c-212d4ad83751)

![image](https://github.com/NANA-2016/Ansible-config-mgt/assets/141503408/d3736862-1a86-4a5b-847c-39d4117f6006)

 ## Creating an ssh agent where we will import our key.
 
 ### COMMAND 
eval `ssh-agent -s`
ssh-add <path-to-private-key>
ssh-add -l command that confirm ssh key has been added into the jenkins Ansible 

## SSH TO MAIN SERVER.

 Using the command below,ssh to the main server as shown below following the command below.
 
 ssh -A ubuntu@public-ip

![image](https://github.com/NANA-2016/Ansible-config-mgt/assets/141503408/a1b7d1fd-a2f5-4c50-bcf3-7c0df633dced)


 UPDATING THE Inventory/dev.yml
[nfs]
<NFS-Server-Private-IP-Address> ansible_ssh_user=ec2-user

[webservers]
<Web-Server1-Private-IP-Address> ansible_ssh_user=ec2-user
<Web-Server2-Private-IP-Address> ansible_ssh_user=ec2-user

[db]
<Database-Private-IP-Address> ansible_ssh_user=ec2-user 

[lb]
<Load-Balancer-Private-IP-Address> ansible_ssh_user=ubuntu

![image](https://github.com/NANA-2016/Ansible-config-mgt/assets/141503408/9a7fbcda-1f9b-425d-af6e-2d3aaa15050d)

---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  become: yes
  tasks:
    - name: ensure wireshark is at the latest version
      yum:
        name: wireshark
        state: latest
   
Creating a common play beook on common.yml
- name: update LB server
  hosts: lb
  become: yes
  tasks:
    - name: Update apt repo
      apt: 
        update_cache: yes

    - name: ensure wireshark is at the latest version
      apt:
        name: wireshark
        state: latest

![image](https://github.com/NANA-2016/Ansible-config-mgt/assets/141503408/606320b4-8726-461f-a99a-889c52d61ea9)

## GIT and VS CODE

Using the vs code , the command git add ., git commit -m"" and git push will be used again and again to update the git hub repository 

for any changes done on the vs code for the servers the folders and files created.

## Ensuring Ansible is running .

cd into Ansible-config-mgt and run the ansible test .

 Using the command :
 
 ansible-playbook -i inventory/dev.yml playbooks/common.yml

. 
![image](https://github.com/NANA-2016/Ansible-config-mgt/assets/141503408/98be7b3d-32ee-40ac-8b1c-04407d1b601b)


Check id wire shark has ben installed using the command :which wireshark on all the servers.


![image](https://github.com/NANA-2016/Ansible-config-mgt/assets/141503408/c1859249-28ff-472f-99ab-c8efbf56363f)





 


