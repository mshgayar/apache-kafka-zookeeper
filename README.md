## 🔗 Links
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com//in/mosalah90/)

# apache-kafka-zookeeper
Apache Kafka and Zookeeper cluster with ansible Based on Centos 7

This Installation is based on Ansible playbook to deploy Kafka and zookeeper cluster
Required Nodes : Three Nodes


      Installation Steps :-

      1) update DNS hostnames on all cluster nodes and your machine that host ansible
      vim /etc/hosts
      192.168.35.174  kafka01.lab.example.com kafka01
      192.168.35.162  kafka02.lab.example.com kafka02
      192.168.35.159  kafka03.lab.example.com kafka03

      2) ansible-playbook -i inventory kafka.yml -t common       
      The purpose of this role is to prepare the apache kafka cluster by installing required packages & java

      3) ansible-playbook -i inventory kafka.yml -t firewall
      The purpose of this role is to create new firewall services for both kakfka and zookeeper

      4) ansible-playbook -i inventory -t "kafka,zookeeper"
      The purpose of those roles is to install & enable and start both kafka and zookeeper services

      Best Regards
      Mohamed Salah
      :)
