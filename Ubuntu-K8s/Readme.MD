## Kubernetes 1 master and 2 worker nodes deployment

** This is to reconfigure sshd_config upon deployment to allow SSH access 
> sudo sed -i "s/.*PasswordAuthentication.*/PasswordAuthentication yes/g" /etc/ssh/sshd_config
> sudo systemctl restart sshd

Note: 
- Sudo seems not neceessary on the script area because it is execute by root. In Progress 
- Set VAGRANT_DEFAULT_PROVIDER=virtualbox to environment variable if default provider is different or config.vm.provider is not set in Vagrantfile

Prepared by: J.A
