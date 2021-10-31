# apache-kafka-zookeeper
Apache Kafka and Zookeeper cluster with ansible

This Installation is based on ansible playbook to deploy kafka and zookeeper cluster 
Required Nodes : Three Nodes
Prerequistes 
a) DNS Name or mapping names through /etc/hosts --> on all three nodes
    - # vim /etc/hosts
    192.168.35.174  kafka01.lab.example.com kafka01
    192.168.35.162  kafka02.lab.example.com kafka02
    192.168.35.159  kafka03.lab.example.com kafka03
    
  Do not forget to update your PC machine with the cluster IPs -->
  On your machine #vim /etc/hosts
      192.168.35.174  kafka01.lab.example.com kafka01
      192.168.35.162  kafka02.lab.example.com kafka02
      192.168.35.159  kafka03.lab.example.com kafka03
      
      Installation Steps :-
      
      1) ansible-playbook -i inventory kafka.yml -t common       
      The purpose of this role is to prepare the apache kafka cluster by installing required packages
      
      2) ansible-playbook -i inventory kafka.yml -t firewall 
      The purpose of this role is to create new firewall services for both kakfka and zookeeper
      
      3) ansible-playbook -i inventory -t "kafka,zookeeper" 
      The purpose of those roles is to install & enable and start both kafka and zookeeper services
      
      Best Regards
      Mohamed Salah
