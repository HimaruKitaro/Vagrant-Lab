## Kubernetes 1 master and 2 worker nodes deployment

**This is to reconfigure sshd_config upon deployment to allow SSH access** 
```
sudo sed -i "s/.*PasswordAuthentication.*/PasswordAuthentication yes/g" /etc/ssh/sshd_config
sudo systemctl restart sshd
```

**Note(s):** 
- Sudo seems not neceessary on the script area because it is executed by root. Modification is in progress
- Set VAGRANT_DEFAULT_PROVIDER=virtualbox to environment variable if default provider is different or config.vm.provider is not set in Vagrantfile

**Sample Output:**
```
vagrant@master-node01:~$ kubectl get nodes -o wide
NAME            STATUS   ROLES    AGE   VERSION   INTERNAL-IP     EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
master-node01   Ready    master   30m   v1.18.2   172.16.100.10   <none>        Ubuntu 18.04.4 LTS   4.15.0-99-generic   docker://19.3.6
worker-node01   Ready    <none>   29m   v1.18.2   172.16.100.11   <none>        Ubuntu 18.04.4 LTS   4.15.0-99-generic   docker://19.3.6
worker-node02   Ready    <none>   29m   v1.18.2   172.16.100.12   <none>        Ubuntu 18.04.4 LTS   4.15.0-99-generic   docker://19.3.6
vagrant@master-node01:~$ kubectl get pods --all-namespaces
NAMESPACE     NAME                                    READY   STATUS    RESTARTS   AGE
kube-system   cilium-5cc66                            1/1     Running   0          29m
kube-system   cilium-bdljk                            1/1     Running   0          29m
kube-system   cilium-nmhlq                            1/1     Running   0          29m
kube-system   cilium-operator-6dd6ddbd78-mhc4t        1/1     Running   0          29m
kube-system   coredns-66bff467f8-6vpwq                1/1     Running   0          29m
kube-system   coredns-66bff467f8-b2tpr                1/1     Running   0          29m
kube-system   etcd-master-node01                      1/1     Running   0          30m
kube-system   kube-apiserver-master-node01            1/1     Running   0          30m
kube-system   kube-controller-manager-master-node01   1/1     Running   0          30m
kube-system   kube-proxy-22xtm                        1/1     Running   0          29m
kube-system   kube-proxy-dsrnj                        1/1     Running   0          29m
kube-system   kube-proxy-w28dp                        1/1     Running   0          29m
kube-system   kube-scheduler-master-node01            1/1     Running   0          30m
vagrant@master-node01:~$
```

**Prepared by: J.A**
