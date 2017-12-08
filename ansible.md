# Ansible

## Use locally

Execute adhoc command

```
# all
# -i "localhost"
#   short for `--inventory` specify comma seperated hosts
# -c local
    short for `--connection` specify connection type
# -m shell
    short for `--module-name` specify module to execute
# -a 'echo hello world'
    short for `--args` specify module arguments
ansible all -i "localhost," -c local -m shell -a 'echo hello world'
```

But nobody wants or does that

```
# create a playbook
touch helloworld.yml

# content
---
- hosts: all
  tasks:
    - shell: echo "hello world"
```

And execute with

```
ansible-playbook -i "localhost," -c local helloworld.yml
```

* **Role** A role is a collection of tasks and templates. For example you can have a role that installs nginx.
* **Inventory** An inventory is a list of hosts, eventually assembled into groups, on which you will run ansible playbooks
* **Playbook** The playbook is the pivot between and inventory and roles. This is where you basically tell Ansible: _please install roles foo, bar and baz on machines alice, bob and charlie_.
* **Task** A task defines a single procedure to be executed, e.g.: install a package
* **Module** A module typically abstracts a system task, like dealing with packages or creating and changing files.
* **Facts** Global variables containing information about the system, like network interfaces
* **Handlers** used to trigger service status changes, like restarting or stopping a service
* **Template**

## Role layout

Role layout is pretty well documented at Ansible website. A role contains several directories. All directories are optional besides tasks. For each directory, the entry point is `main.yml`. Thus, the only compulsory file in a role is `tasks/main.yml`.

```
ansible-foobar/
├── defaults
│   └── main.yml
├── files
├── handlers
│   └── main.yml
├── meta
│   └── main.yml
├── tasks
│   ├── check_vars.yml
│   ├── foobar.yml
│   └── main.yml
└── templates
    └── foobar.conf.j2
```

## Best practices

https://github.com/enginyoyen/ansible-best-practises
http://docs.ansible.com/ansible/latest/playbooks_best_practices.html

kkk
