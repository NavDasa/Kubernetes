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

Note: while we are configuring with centos-master or centos-node if it is not properly configuring in the place of centos-master usse the IP Address of the centos-master & In the place of centos-nodes use the IP Address what we have given as per the requirement.

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
      
2. Install and configure Master controller: we has to be in the Root user.
     1. Go the configuration file by using the below commands:
            
            cd /etc/kubernetes
            ll
           
     2. we can see the configuration file here change and add the configuration file in the controller manager, schedular & proxy that kube master has to run in the centos-master node in 8080 and also add the following etcd_servers along kubemasters is shown below:   
            
            vim config
            
                  # How the control manager, schedular & proxy and api server.
                  
                 KUBE_MASTER="--master=http://centos-master:8080"
                 
                 KUBE_ETCD_SERVERS="--etcd-servers=http://centos-master:2379"
                 
            
      3. Now go to the etcd configuration file:
             
             cd /etc/etcd
             ll
      
      4. Change and edit the etcd configuration file:
             
             vim etcd.conf
             
                  # In the Member and cluster sections edit the etcd clients url and etcd advertise client urls from local host to 0.0.0.0
                  
                  ETCD_LISTEN_CLIENTS_URLS="http://0.0.0.0:2379"
                  
                  ETCD_ADVERTISE_CLIENTS_URLS="http://0.0.0.0:2379"
      
      5. Go back to the kubernetes directory and edit the api-server:
                  
                  cd /etc/kubernetes/
                  ll
      
      6. Edit the api-server: 
      
                  vim apiserver
                  
      7. Edit and change as fallows i.e it has to be like below for the fallowing and the remaining all are same:
      
                  # The address of the local server listen to
                  KUBE_API_ADDRESS="--address=0.0.0.0"
                  
                  # The port on the local server to listen on.
                  KUBE_API_PORT="--port=8080"
                  
                  # Port minions listen to
                  KUBELET_PORT="--kubelet-port=10250"
                  
                  # Comma separated list of nodes in the etcd cluster
                  KUBE_ETCD_SERVICE_ADDRESSES="--service-cluster-ip-range=10.254.0.0/16"
                  
                  # default admission control polices
                  # KUBE_ADMISSION_CONTROL="--admission-control=NamespaceLifecycle.NamespaceExists.LimitRanger.SecurityContext.ServiceAccount.Restore.ResourceQuota"
                  
    
      8. Make sure after changing and saving all this we have to enable the four services as fallows:
      
                  systemctl enable etcd kube-apiserver kube-controller-manager kube-scheduler
                  
                  systemctl start etcd kube-apiserver kube-controller-manager kube-scheduler
                  
                  systemctl status etcd kube-apiserver kube-controller-manager kube-scheduler | grep "(running)" | wc -l
                  
    Note: The above configuration as to be done only in the Master only.
    
    
 3. Install and Configure Nodes(Minions) : 
 
 We has to do This configuration only in the Node1, Node2, Node3 ..n all the Nodes only depending on how many nodes you are taking.
   1. Go the configuration file by using the below commands:
            
            cd /etc/kubernetes
            ll
            
   2. we can see the configuration file here change and add the configuration file in the controller manager, schedular & proxy that kube master has to run in the centos-master node in 8080 and also add the following etcd_servers along kubemasters is shown below:   
     
            vim config
            
                  # How the control manager, schedular & proxy and api server.
                  
                 KUBE_MASTER="--master=http://centos-master:8080"
                 
                 KUBE_ETCD_SERVERS="--etcd-servers=http://centos-master:2379"
                 
                 
   3. Go back to the kubernetes directory and edit the kubelet:
                  
                  cd /etc/kubernetes/
                  ll
      
   4. Edit the kubelet: 
      
                  vim kubelet
                       
   5. See the kubelet configuration file has to be as fallows in some places & remaining all are same:
   
                  # The address for the info server to serve on (set to 0.0.0.0 or "" for all interfaces)
                  KUBELET_ADDRESS="--address=0.0.0.0"
                  
                  # The port for the info server to serve on
                  KUBELET_PORT="--port=10250"
                  
                  # You may leave this blank to use the actual hostname
                  KUBELET_HOSTNAME="--hostname-override=centos-minion1"
                  
                  # location of the api-server
                  KUBELET_API_SERVER="--api-server=http://centos-master:8080"
                  
                  # pod infrastructure container
                  # KUBELET_POD_INFRA_CONTAINER="--pod-infra-container-image=re.....................everything are same in the configfile file
                  
   6. Make sure after changing and saving all this we have to enable the three services as fallows:
      
                  systemctl enable etcd kube-proxy kubelet docker
                  
                  systemctl start etcd kube-proxy kubelet docker
                  
                  systemctl status etcd kube-proxy kubelet docker | grep "(running)" | wc -l
                  
                  
   7. we have to make sure that docker is running currently are not by using the below commands and also check by pulling the hello-world image.
   
                  docker images
                  docker version
                  docker pull hello-world
                  docker images
                  docker run hello-world
                  docker ps
                  docker ps -a
                  
   
4 . Kubectl: Exploring our Environment ( Main Untility):
 
   1. Basically kubectl controlles & manages the cluster manager we can also see the kubectl man page by using below command:
                  
                  man kubectl
                  
   2. we can also check that what nodes are registerd & also can see the man page of kubectl-get by using the below command:
      
                  kubectl get nodes
                  
                  man kubectl-get
                  
   3. If we want to describe the nodes & also describes the nodes in the JSON format & also says that they are ready=True:
                  
                  kubectl describe nodes
                  
                  kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="ExternalIP")].address)'
                  
                  kubectl get nodes -o jsonpath='{range .items[*]}{@.metadata.name}:{range @.status.conditions[*]}{@.type}={@.status};{end}{end}'| tr ';' "\n"  | grep "Ready=True"
                  
   4. Finally we know that an Nodes consists of pods or services which they don't have Now, So If we use below command It shows Nothing:   
     
                  kubectl get pods
                  


Creating and Deploying Pods:

   1. Below what we do exactly is that we create a YAML file and name it as nginx.yaml in a Directory called Build.In order to create a Pod contains an nginx container. So after creating Pod this can be deployed in one Node, So that we can see the same nginx container. 
   
      In this senario we use one centos-Master and one centos-Node. Below is the commands to start with:
   
                  mkdir Builds
                  ll
                  cd Builds
                  
                  vim nginx.yaml              
                  
      Below is the YAML file that we has to paste the below content
      
                  apiVersion: V1
                  kind: Pod
                  metadata:
                    name: nginx
                  spec:
                    containers: 
                    - name: nginx
                      image: nginx:1.7.9
                      ports:
                      - containerPort: 80
                      
      In order to create the Pod Just Run the below command: This creation will create the pod that contains an nginx container in both Master and one Node we are using.
      
                  kubectl create -f ./nginx.yaml 
      
      Also we can check is the container is running in the Minion(Node) as well by using the below command, by going to the Node:
      
                  docker ps
                  
      If we want to Know what the Pods are running or Particular pod by using the below command in the Master :
      
                  kubectl describe pods
                  kubectl describe pod nginx
                 
      Note: Here we can see the IP address, So in order to connect to that Particular IP address externaly by using the command "ping 172.17.0.2" we thrown an error, because we don't have route externally to that Pod, in order to rectify that we will create an other resources with our pod in our environment so that we can ping to that IP Address. So the name of the resource is busybox & run this busybox image within the environment, By using the below commands:
 
                  kubectl run busybox --image=busybox --restart=Never --tty -i --generator=run-pod/v1
                  
      we will be with the busybox container So paste the below command:
      
                  wget -q0- http://172.17.0.2
                  
      Here we get the access for this Pod internally, we can see the html page of nginx. 
      
      Next we can also delate the both image of Busybox & also Particular Pod in the master so that it will delate automatically in the Nodes(Minions) as well by using the below commands:
      
                  kubectl delete pod busybox
                  kubectl delete pod nginx
       
      In order to get nginx access externally, we has to first do the port forward, in order to do that create the nginx pod again and do port farword: 
      
                  kubectl get pods
                  kubectl create -f ./nginx.yaml
                  
                  kubectl port-forward nginx :80 &
                         (& - represents runit in the background)
                         (we can also do 8080:80 if we need)
                        
                  wget -q0- http://localhost:34853 
                  
  
      Note: Now we get the nginx service externally by using the above wget command.
      
      
                  
                  
                  
      
      
      
                  
 
                  
                  
                  
                  
                  
              
      
             



