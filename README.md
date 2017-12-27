# Kubernetes
Kubernetes Architecture &amp; Commands

In Kubernetes we have a Master controller that controls the Nodes. and each Nodes contains a Docker Engine & Pods. And If we go into deep that each Pod contains Number of containers. 

--> Pods: It contains a Unique IP Address. So that the traffic can distribute in the containers. Can also share the volumes, disk utilizations, Finally the pod management is done through API or delegated through controller.
--> Labels: Clients can manage "Key value pairs". Used for Identification of configuration and management system.
--> Selectors: Represent quries that made againest labels. They resolve to the corresponding macthing objects. The labels and selectors are the primary way where grouping is done in kubernetes. 




