# I2T_SOCLE_RSTUDIO
=========

Description
------------
Install Rstudio Server Professional on a server

Requirements
------------
- Install R before.
- Get Rstudio Server Pro RPM

Role Variables
------------
You need to init this variables :
server_repo_url, server_mirror_repo_url, java_path_server, r_java_home, entity_name, role_create_fs_list

Dependencies
------------
You have to get the role to manage fs/lv.

Example Playbook
------------

main.yml :

```
---
- name: "Create all FS"
  roles:
    - "role_manage_fs"
  vars:
    state: "create"


- name: "Launch role to install RSTUDIO"
  hosts: "EDGE_DS"
  roles:
    - "I2T_SOCLE_RSTUDIO"
  vars:
    state: "role_rstudio_install" #Install Rstudio
    state: "role_rstudio_config" #Config Rstudio
    state: "role_rstudio_correction" #Correct the bug with identication with blank
    state: "role_rstudio_remove" #Remove Rstudio
    state: "role_rstudio_whole_install" #Install + Config + Correction
```

Vars:
```
---
entity_name: "bsc"
role_create_fs_list: 
  - { name: "rstudio",
      mntp: "/applis/rstudio",
      fstype: "ext4",
      size: "1g",
      device: "ApplisVg" ,
      owner: "rstudio-server",
      group: "rstudio-server",
      mode: '700'
     }

### JAVA
java_path_server: "/usr/java/jdk1.8.0_121/jre/lib/amd64/server"
r_java_home: "/usr/java/jdk1.8.0_121/jre/"

### RSTUDIO
rstudio_port: "8839"
```


Launch playbook without extra-vars:
```bash
ansible-playbok main.yml -i <your_inventory>
```

Launch playbook with extra-vars:
```bash
ansible-playbok main.yml -i <your_inventory> --extra-vars="state:role_rstudio_<install,config,correction,remove,whole_install>"
```

To avoid a HA Rstudio, you can use "skip-tags:HA" when you launch your playbook

Author
------------
Franck VIEIRA
