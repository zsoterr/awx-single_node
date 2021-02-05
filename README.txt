# awx-single_node
Support AWX single node deployment

Short description:
We used playbooks to prepare an awx node.
Pre-requirements:
- based on Centos v7,
- techical user which is able to login to planned awx node using ssh and become to root without password
OR
you can run the playbooks, directly on the target node
- change the 'defaultuser' username in playbooks

The playbooks prepare the target node:
- prep-new-awx-install-docker.yml,
- prep-new-awx-nodes.yml

After the preparation you can run the AWX v15 single node "offical installer":
on the prepared awx node:
wget https://github.com/ansible/awx/archive/15.0.1.tar.gz
tar xzf 15.0.1.tar.gz
cd awx-15.0.1/installer/
ansible-playbook -i inventory install.ym

check:
# docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED              STATUS              PORTS                  NAMES
e6d2d6acf3d1   ansible/awx:15.0.1   "/usr/bin/tini -- /u…"   About a minute ago   Up About a minute   8052/tcp               awx_task
8b8a71deff2b   ansible/awx:15.0.1   "/usr/bin/tini -- /b…"   About a minute ago   Up About a minute   0.0.0.0:80->8052/tcp   awx_web
d6b8881373d4   redis                "docker-entrypoint.s…"   2 minutes ago        Up About a minute   6379/tcp               awx_redis
6f4f79811e49   postgres:10          "docker-entrypoint.s…"   2 minutes ago        Up About a minute   5432/tcp               awx_postgres

visit the awx web ui:
http://IP_ADD_of_node/#/home
