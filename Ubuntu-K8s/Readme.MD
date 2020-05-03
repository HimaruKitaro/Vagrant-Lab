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

**Deploy Metric Server and Dashboard**
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.3.6/components.yaml
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
```

**Create Sample User to access Dashboard**
```
$ cat admin-user.yml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard

---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard

$ kubectl apply -f admin-user.yml
$ kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
$ kubectl proxy
```
> Dashboard is available at http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/ and token extracted from command above describing secrets.

**Prepared by: J.A**
