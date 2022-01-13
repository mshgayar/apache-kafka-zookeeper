## 🔗 Links
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com//in/mosalah90/)

# apache-kafka-zookeeper
Apache Kafka and Zookeeper cluster with ansible Based on Centos 7

This Installation is based on Ansible playbook to deploy Kafka and zookeeper cluster
Required Nodes : Three Nodes


      Installation Steps :-
      1) Infra Prepration
        
        a) ssh key to the three nodes
        b) create ssh and copy the public key to the Kafka cluster nodes
        c) configure the inventory file with the ssh key ~/.ssh/ssh-key
        d) configure DNS to map IP with hostname for all Kafka nodes , 
        note i am using .lab.example.com as domain for my Kafka cluster (nodes) which is must be replaced with the production DNS domain name.
        
        e) ensure correct host-names to be replaced in below files :-
        - ansible inventory file
        - roles/zookeeper/templates/zoo.cfg.j2 
        - replace *.lab.example.com with the new host names
        - roles/kafka/templates/server.properties.j2
        replace *.lab.example.com with the new host names
        
      vim /etc/hosts
      192.168.35.174  kafka01.lab.example.com kafka01
      192.168.35.162  kafka02.lab.example.com kafka02
      192.168.35.159  kafka03.lab.example.com kafka03

      2) ansible-playbook -i inventory kafka.yml -t common       
      The purpose of this role is to prepare the apache kafka cluster by installing required packages & java
      
      Role Tasks:
            a) install java & required packages for Kafka servers (java-1.8.0-openjdk.x86_64 , vim , wget, git , firewalld,bind-utils,git,httpd-tools)
            b) update /etc/bashrc
                 export JRE_HOME=/usr/lib/jvm/jre
                 export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk
                 PATH=$PATH:$JRE_HOME:$JAVA_HOME
                 
            c) Set nofile limits /etc/security/limits.d/90-nproc.conf
                    "* soft nproc 32768"
                    "* hard nproc 32768"
                    
            d) Set nofile limits /etc/security/limits.conf
                  "* soft nofile 32768"
                  "* hard nofile 32768"
                  
            e) Set swappiness to 1
                  vm.swappiness=1
                  
            f) Disable Transparent huge pages untill reboot
                  echo never > /sys/kernel/mm/transparent_hugepage/enabled
                  echo never > /sys/kernel/mm/transparent_hugepage/defrag

      3) ansible-playbook -i inventory kafka.yml -t firewall
      The purpose of this role is to create new firewall services for both kakfka and zookeeper

      4) ansible-playbook -i inventory -t "kafka,zookeeper"
      The purpose of those roles is to install & enable and start both kafka and zookeeper services
      
      Kafka Role Tasks :
            1) Create user and group "kafka" to run own kafka configurations directory & run kafka and zookeeper services
            2) Download apache kafka and zookeeper tarball file
            3) Extract the tarball file kafka_2.12.2.1.0.tgz and copy its contents to /opt/kafka
            4) Set permissions on /opt/kafka to be owned by "kafka" user and Group
            5) Create kafka log directory : /var/log/kafka-logs & set permissions to be owned by "Kafka" user and Group
            6) Create zookeeper data directory : /var/lib/zooKeeper & set permissions to be owned by "Kafka" user and Group
            7) Install kafka service & run it as daemon /etc/systemd/system/kafka.service
            8) Configure kafka "configuration file" /opt/Kafka/config/server.properties
            9) Reload systemctl daemon
      
      Note :  before running the ansible kafka playbook , edit the file roles/kafka/templates/server.properties.j2
      
      5) ansible-playbook -i inventory -t zookeeper
      The purpose of those roles is to install & enable and start zookeeper service
      
      Zookeeper Role Tasks:
            1) Install zookeeper daemon service at /etc/systemd/system/zookeeper.Service
            2) Configure zookeeper configuration file /opt/kafka/config/zookeeper.properties
            3) Configure broker id at /var/lib/zookeeper/myid
            4) reload systemctl daemon

      Note: before running the ansible playbook , edit the below files
            1) vim roles/kafka/templates/server.properties.j2
                  - replace “lab.example.com“ with the new domain name
                  - ansible inventory file must have the full DNS for the three nodes
                  
            2) vim roles/zookeeper/templates/zoo.cfg.j2
                  - ensure the correct name of the nodes (correct DNS hostname )
                  
      make sure kafka and zookeeper services are up and running
   

      Finally To deploy the full command with one command 
      
      ansible-playbook -i inventory kafka.yml

      You can contact me on mshgayar@gmail.com
      Best Regards
      Mohamed Salah
      :)
