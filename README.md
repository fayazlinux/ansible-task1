# ansible-task1
!!!!Run Docker Containr via Ansible!!!!

Ansible Configure in RED HAT  8:
###############################

NOTE: Configure Yum Local Before Going a Head. 

1. Install the Ansible with the help of pip command !!! 
   
   pip3  install ansible 

2. Create confige file where  Ansbile will read the Inventroy . 
   
   config file = /etc/ansible/ansible.cfg (create if its not created).

3. Make the inventory by add mention below line in ansible.cfg 
    
   [defaults]
   inventory = /etc/ansible/hostfile
   host_key_checking = false

4. ADD Managed Node  in /etc/ansible/hostfile  

   vi /etc/ansible/hostfile   
   #insert these line in above file .(change the all parameter accordingly)
   61.28.1.62   ansible_ssh_user=root  ansible_port=2222   ansible_ssh_pass=123@123 
   
5. Install SSHPASS rpm in Controllar Node (Ansible Node)
   
   yum install epel-release -y 
   yum install sshpass   -y 

6. Check the connetivity 
   
    ansible -m ping all

        OUTPUT: 
          61.28.1.62 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false,
    "ping": "pong"
}


TASK START : 
############

1.Configure Docker Yum Repo: 

 - name: Add Docker Repo
      yum_repository:
       name: Dockerrepo
       description: Dockerrepo
       baseurl: https://download.docker.com/linux/centos/7/x86_64/stable/
       gpgcheck: no

2. Install the Docker 

 - name: Install Docker
      command: "yum install docker-ce -y --nobest"

3. Start The Docker Services : 

 - name: Start service Docker
      service:
       name: docker
       state: started

4. Install Python on Remote Machine : 

  - name: install the Python Remote System
      yum:
       name: python36

5. Install Docker Dependence: 

   - name: Install Docker-py
      command: "pip3 install docker-py"

6. Create A Folder on  Remote Machine :

   - name: create web folder Remore System
      file:
       path: /web
       state: directory

7.  Copye index.html file on Remote Folder : 

   - name: copying file into remote server
      copy:
       src: /task1/index.html
       dest: /web


8. Pull The Docker image : (You can USE your Own ) 

   - name: pull an image
      docker_image:
       name: fayazlinux/fayazhttpd:v30
       source: pull

9. Create Container and Expose to World :

     - name: Create a data container
      docker_container:
       name: mydata
       image: fayazlinux/fayazhttpd:v30
       ports:
        - "80:80"
       volumes:
       - /web:/var/www/html/

10. Complete Code you can See on Git hub link.
