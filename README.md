# Project set up
* create an inventory file (e.g. hosts or hosts.yaml) that holds the remote hosts that ansible will handle.
```yaml
docker-servers:
  hosts:
      dockervm:
          ansible_host: appserver # <-- name in the config file where docker will run
dbservers:
    hosts:
      dbserver-vm:
          ansible_host: dbserver # <-- name in the config file where postgres db will run (native)
      azure-db-server:
          ansible_host: devopsvm # <-- name in the config file where postgres db will run (native)

backendservers:
    hosts:
      backend-server:
        ansible_host: devops-gcloud # <-- name in the config file where backend spring will run (native)

frontendservers:
    hosts:
      frontend-server:
          ansible_host: devopsvm2 # <-- name in the config file where frontend vue will run (native)
```

* to test if all hosts are accesible, run
```bash
ansible -m ping all
```
* to test if a group of hosts are accesible, run
```bash
ansible -m ping all <group-name>
```

* run playbook for postgres
```bash
ansible-playbook playbooks/postgres.yaml -l azure-db-server
```

* run playbook for spring (native)
```bash
ansible-playbook playbooks/spring.yaml -l backend-server  -e dbvm_ip=<your_postgres_vm_ip>```
```

* run playbook for vue (native)
```bash
ansible-playbook playbooks/vuejs.yaml -l frontend-server -e frontendvm_ip=<your_frontend_vm_ip> -e backendvm_ip=<your_backend_vm_ip> 
```

* run playbook for docker
```bash
ansible-playbook -i ~/workspace/ansible-aimodosia/hosts.yaml -l dockervm ~/workspace/ansible-aimodosia/playbooks/spring-vue-docker.yaml
```

## Get host basic info
```bash
ansible-playbook -l <hostname> playbooks/hostvars_and_facts.yml
```

## postgres from ansible-galaxy
install postgresql role
```bash
ansible-galaxy install geerlingguy.postgresql
```