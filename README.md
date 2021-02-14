# PiHole on Kubernetes

This works on a bare metal k8s cluster with metallb. I have not attempted dhcp with this. I opted to specify a load balancer ip myself rather than allow metallb to select one from its pool so that it is predictable for assignment in my dhcp service.

Persistent configuration is handled via configmaps, including local DNS entries. It should suffice to edit as appropriate (custom-list configmap and the addressing in 03-service.yaml should be set at a minimum) and `kubectl apply -f .`

```
[root@kube0 pihole-kubernetes]# kubectl apply -f .
namespace/k8s-pihole created
configmap/pihole-adlists created
configmap/pihole-regex created
configmap/pihole-custom created
configmap/pihole-env created
deployment.apps/pihole-deployment created
service/pihole-web-service created
service/pihole-dns-service created
service/pihole-tcp-dns-service created
[root@kube0 pihole-kubernetes]# kubectl get pods -n k8s-pihole
NAME                                READY   STATUS    RESTARTS   AGE
pihole-deployment-d797f5d8f-8bqvd   1/1     Running   0          13s
pihole-deployment-d797f5d8f-m99bw   1/1     Running   0          13s
[root@kube0 pihole-kubernetes]# kubectl get svc -n k8s-pihole
NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)        AGE
pihole-dns-service       LoadBalancer   10.97.43.17      192.168.0.192   53:31565/UDP   23s
pihole-tcp-dns-service   LoadBalancer   10.110.255.178   192.168.0.192   53:30521/TCP   23s
pihole-web-service       LoadBalancer   10.97.171.142    192.168.0.192   80:30203/TCP   23s
[root@kube0 pihole-kubernetes]# dig @192.168.0.192 example +short
192.168.0.1
[root@kube0 pihole-kubernetes]# dig @192.168.0.192 google.com +short
216.58.206.110
[root@kube0 pihole-kubernetes]#
```
