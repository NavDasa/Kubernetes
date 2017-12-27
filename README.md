# Kubernetes
Kubernetes Architecture &amp; Commands

In Kubernetes we have a Master controller that controls the Nodes. and each Nodes contains a Docker Engine & Pods. And If we go into deep that each Pod contains Number of containers. 

1. Pods: It contains a Unique IP Address. So that the traffic can distribute in the containers. Can also share the volumes, disk utilizations, Finally the pod management is done through API or delegated through controller.
2. Labels: Clients can manage "Key value pairs". Used for Identification of configuration and management system.
3. Selectors: Represent quries that made againest labels. They resolve to the corresponding macthing objects. The labels and selectors are the primary way where grouping is done in kubernetes. 
4. Controller: Used to manage Pods within the cluster and container within the pods with the help of Replication controler. With the help of this replication controller if any container fails in the pod, it automatically replaces the new controller. We do have a Job controler that controls pods to completion of the batch Jobs. Demaon set controler that enforces an 1 to 1 ratio of Pods to Nodes. Also manage the cluster. Each set of pods that any controller manages, is determined by the label selectors. 
5. Service: In kubernetes each service can handle a routing with the static IP for each pod as well as load balancing(Round robin based) connection to that service among the pods that match the label selector indicated. By default, the service is only Exposed inside a cluster, it is also exposed outside the cluster as needed.

Introduction to YAML:
1. YAML(Yet another Markup language) (Human readable data serialization format).
2. Based on Idententation, also compiling & running complex applications. 


Kubernetes setup and Configurations:

1. Packages and Dependencies:
     First thing we have to download ntp service in all of our Master and Minions(Nodes).
      1. yum install -y ntp
      2. systemctl enable ntpd && systemctl start ntpd && systemctl status ntpd
      3. vim /etc/hosts
              # Paste the IP address & name of the Nodes in order to communicate.
              173.31.24.15 centos-master1
              173.31.25.26 centos-Node1
              173.31.26.58 centos-Node2
              173.31.28.65 centos-Node3
      4. We can also verify it is configured correctly or not by ping centos-Node1 in Master, and ping centos-master in the node.
      
             
             



