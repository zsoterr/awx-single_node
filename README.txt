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

After the preparation you can run the AWX v16 single node "offical" installer:
on the prepared awx node:
 wget https://github.com/ansible/awx/archive/16.0.0.tar.gz 
 tar xzf 16.0.0.tar.gz
 cd awx-16.0.0/installer/installer
run this command before you run the deployment:
 sed -i 's/create_preload_data=True/create_preload_data=False/g' inventory 
// reason: there is a bug in v16 deployment, more information: please visit https://github.com/ansible/awx/issues/8863
run the deployment:
 ansible-playbook -i inventory install.yml
visit the AWX Web UI and wait until you get the "normal" login screen
run this command in shell on the AWX node:
 # docker exec -i awx_task bash -c " /usr/bin/awx-manage create_preload_data"
   Default organization added.
   Demo Credential, Inventory, and Job Template added.
   (changed: True)
This command will create the "default" demo project. You can check it, visiting http://IP_Address_of_AWX_Server/#/projects 

check:
# docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED              STATUS          PORTS                  NAMES
7b1b4972a91c   ansible/awx:16.0.0   "/usr/bin/tini -- /u…"   About a minute ago   Up 57 seconds   8052/tcp               awx_task
c140006b7626   ansible/awx:16.0.0   "/usr/bin/tini -- /b…"   About a minute ago   Up 56 seconds   0.0.0.0:80->8052/tcp   awx_web
01b1b0cd5d43   redis                "docker-entrypoint.s…"   About a minute ago   Up 58 seconds   6379/tcp               awx_redis
2f2e2ab555f9   postgres:10          "docker-entrypoint.s…"   About a minute ago   Up 49 seconds   5432/tcp               awx_postgres
