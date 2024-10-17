# connect controller to nodes
## create 3 instances, 1 controller and 2 nodes
1. create security group for `controller: control-sg`
2. create security group for `nodes: client-sg`
   
change inbound group roles and  security group roles as shown in fig
>in nodes instances select security group roles> source> control-sg. so these nodes will connect to controller instance

![image alt](https://github.com/KarampudiKarthik/ansible-zero-to-hero/blob/main/my/images/Capture.PNG?raw=true)

> changes the names of nodes to node 1,2,3 for 3 nodes

## install ansible in controller instance
