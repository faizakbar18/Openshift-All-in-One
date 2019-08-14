# Openshift-All-in-One
How To Install Openshift All in One using Ansible on Centos 7

what that means is that all of the services required for OKD to function (master, node, etcd, etc.) will all be installed on a single host. 

__we used ostk.adfazmedia.com as DNS__

***Make sure hostname same as DNS used***

**Please do use a clean CentOS system, the script installs all necesary tools and packages including Ansible, container runtime, etc.**

## Installation
1. Login as root.
2. Install OpenShift Origin 3.11 repository and Docker and so on.
```
yum -y install centos-release-openshift-origin311 epel-release docker git pyOpenSSL nano
```
3. Start and Enable Docker
```
systemctl start docker
systemctl enable docker
```
4. Set SSH keypair with no pass-phrase.
```
ssh-keygen -q -N ""
ssh-copy-id ostk.adfazmedia.com
```
5. Run Ansible Playbook for setting up OpenShift Cluster.
- Install openshift-ansible
```
yum -y install openshift-ansible
```
- Downgrade ansible to 2.6.x version
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
pip install ansible==2.6.9
```
- Edit ansible hosts like the file [hosts](hosts)
```
nano /etc/ansible/hosts
```
- Run Prerequisites Playbook
```
 ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/prerequisites.yml
```
- Run Deploy Cluster Playbook
```
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/deploy_cluster.ym
```

**When done will like this**

![Success Image](https://image.prntscr.com/image/ktQmEnapRMS2MNWuKuF6lg.png)

6. Check to make sure the installation was successful.

**Note: First time run the response will be long, just wait the loading proccess**

- Accessing url, url will like `https://DNS:8443/console`
```
https://ostk.adfazmedia.com:8443/console
``` 
- or with cli.
```
oc get nodes
oc get pods
```

## Add User
After installation finished and the url can be accessed, we can't login, because no one user have been added yet, so we must add it.
1. Login as root
2. Add a new user with HTPasswd.

```
htpasswd /etc/origin/master/htpasswd faizakbar18
```
Change __faizakbar18__  with the user you want to create

3. Login to OpenShift Cluster with a user just added with HTPasswd.


### Reference
I was try and error many time, but the method above that I used and was successful.

- https://www.server-world.info/en/note?os=CentOS_7&p=openshift311
- https://github.com/gshipley
