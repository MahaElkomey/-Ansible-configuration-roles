# Ansible-Configuration-Roles
A server configuration by installing nginx and create templates, copy files to each host server, and start nginx as a service.
## Features
### 1. Create a Role called Web :
``` sh
- name: play1
  hosts: web
  gather_facts: false
  roles:
    - web

```
### 2. Installing a package :
``` sh
- name: task 1 install nginx
  apt:
    name: "{{ package_name }}"
    state: present
  notify: copy_handler

``` 
### 3. Copying a file from controller to host using template :

``` sh
- name: temp_handler
  template:
     src: "{{ template_file }}"
     dest: /var/www/html/index.html
  notify: start_service_handler
  
```
### 4. Copying a list of files from controller to host using loop :

``` sh
- name: copy_handler
  copy:
    src: "{{ item }}"
    dest: /var/www/html/
  loop: "{{ files_list }}"
  notify: temp_handler 
  
```
### 5. Restart the service of the installed package using Handlers chaining :

``` sh
- name: start_service_handler
  command:
    cmd: service nginx start 
  
```
-------------------------------------------------------------------------------------------------------------------------------------------------------
## Apply the role_web playbook :
``` sh
sudo ansible-playbook role_web.yaml --ask-become-pass 

```

<img width="897" alt="Untitled" src="https://user-images.githubusercontent.com/47718954/231604635-0b3c6ccc-f0c7-4dd0-bfb5-10681e6b378a.png">

## Try to curl to one of the servers :
``` sh
curl 172.17.0.3 

```
<img width="301" alt="Untitled2" src="https://user-images.githubusercontent.com/47718954/231604809-62d840b6-b5f5-4010-8faa-83fa1090e232.png">

