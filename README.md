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
      1. First thing we have to download ntp service in all of our Master and Minions(Nodes).
      
               yum install -y ntp
      2. We have to enable & start the ntpd services. 
               
               systemctl enable ntpd && systemctl start ntpd && systemctl status ntpd
      
      3. vim /etc/hosts
             
             # Paste the IP address & name of the Nodes in order to communicate.
              
              173.31.24.15 centos-master1
              173.31.25.26 centos-Node1
              173.31.26.58 centos-Node2
              173.31.28.65 centos-Node3
              
      4. We can also verify it is configured correctly or not by using the below commands: 
               
               ping centos-Node1 in Master 
               ping centos-master1 in the node
               
      5. vim /etc/yum.repos.d/virt7-docker-common-release.repo 
               
               # Paste the below content in both Minions(Nodes) and Master for the etcd to communicate
               
               [virt7-docker-common-release]
               name=virt7-docker-common-release
               baseurl=http://cbs.centos.org/repos/virt7-docker-common-release/x86_64/os/
               gpgcheck=0

      6. Update the Environment in both Master and Nodes. 
               
                    yum update 
                    
      7. We have to enable repo and install the etcd for kuberntes & also we have to install docker also by using the command: 
          
               yum install --enablerepo=virt7-docker-common-release etcd kubernetes docker
          
     Note: Everything relates to packages and depencies has should be done in both Master and Nodes
      
2. Install and configure Master controller:
     1. 
             



