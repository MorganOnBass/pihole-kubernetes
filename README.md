# PiHole on Kubernetes

This works on a bare metal k8s cluster with metallb. I have not attempted dhcp with this. I opted to specify a load balancer ip myself rather than allow metallb to select one from its pool so that it is predictable for assignment in my dhcp service.

Persistent configuration is handled via configmaps, including local DNS entries. It should suffice to edit as appropriate (custom-list configmap and the addressing in 03-service.yaml should be set at a minimum) and `kubectl apply -f .`
