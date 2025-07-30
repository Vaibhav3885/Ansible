# Ansible

### ✅ Ansible Deep Dive (Simple English)

#### 🔸 1. What is Ansible?
Ansible is an open-source automation tool used for:

Configuration management (install software, set configs)

Application deployment (automate app release)

Provisioning (create VMs, users, etc.)

Orchestration (run tasks in sequence)

It uses YAML-based playbooks and connects to servers using SSH — no need to install an agent on target servers.

#### 🔸 2. Key Components of Ansible
Term	Meaning
Inventory	List of servers (hosts) you want to manage
Modules	Reusable scripts (e.g., install package, copy file)
Playbook	YAML file with tasks
Task	A single action (like install nginx)
Roles	Organized directory structure for playbooks
Facts	System info collected from managed nodes

🔸 3. Example Ansible Inventory
```
[webservers]
192.168.1.10
192.168.1.11

[dbservers]
db01.example.com
```
#### 🔸 4. Sample Playbook
```
- name: Install Nginx on web servers
  hosts: webservers
  become: true
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
```
#### 🔸 5. How to Run a Playbook
```
ansible-playbook -i inventory.ini nginx-install.yml
```
#### 🔸 6. Variables in Ansible
Used to avoid hardcoding values.

```
vars:
  pkg_name: nginx

tasks:
  - name: Install package
    apt:
      name: "{{ pkg_name }}"
      state: present
```
#### 🔸 7. Ansible Roles
Roles are a way to organize your code.

Directory structure:

```
roles/
  web/
    tasks/
      main.yml
    handlers/
    templates/
    vars/
```
Used like this:

```
- hosts: webservers
  roles:
    - web
```
#### 🔸 8. Handlers
Run only when notified by a task.

```
tasks:
  - name: Update config
    copy:
      src: config.cfg
      dest: /etc/myapp/config.cfg
    notify: Restart app

handlers:
  - name: Restart app
    service:
      name: myapp
      state: restarted
```
#### 🔸 9. Conditionals & Loops
When condition:

```
- name: Install package
  apt:
    name: apache2
  when: ansible_os_family == "Debian"
```
Loop example:

```
- name: Install multiple packages
  apt:
    name: "{{ item }}"
  loop:
    - git
    - curl
```
#### 🔸 10. Tags
Run selected tasks using tags.

```
- name: Install nginx
  apt:
    name: nginx
    state: present
  tags: install
```
Run with:

```
ansible-playbook playbook.yml --tags "install"
```

## 🎯 Real-World Ansible Interview Questions & Answers
#### 🔸 Q1. What is Ansible, and how is it different from other tools like Chef/Puppet?
Answer:
Ansible is agentless and uses SSH to connect. It's simpler than Chef/Puppet and uses YAML files. No separate master-server setup is needed.

#### 🔸 Q2. What are Playbooks and how do you write one?
Answer:
Playbooks are YAML files with tasks that Ansible runs. Each task defines an action, like installing a package or copying a file.

#### 🔸 Q3. What is the use of Ansible roles?
Answer:
Roles help organize your playbook into folders like tasks, handlers, templates, making the code clean and reusable across projects.

#### 🔸 Q4. How do you manage multiple environments (dev/stage/prod) in Ansible?
Answer:

Use different inventory files for each environment

Use variable files like dev.yml, prod.yml

Use conditionals or group_vars/host_vars to manage env-specific settings

#### 🔸 Q5. How can you run only selected tasks in a playbook?
Answer:
Use tags in the playbook, and run with --tags.

#### 🔸 Q6. What are handlers and when are they triggered?
Answer:
Handlers run only when a task tells them to run using notify. Mostly used to restart services after a config change.

#### 🔸 Q7. What’s the difference between copy and template module?
Answer:

copy just copies a file

template can use variables inside a Jinja2 file (.j2)

#### 🔸 Q8. How do you secure secrets in Ansible?
Answer:
Use Ansible Vault to encrypt secrets like passwords and keys.

```
ansible-vault encrypt secrets.yml
```
#### 🔸 Q9. What are facts in Ansible?
Answer:
Facts are collected system info like IP address, OS, memory, etc., which can be used in playbooks.

```
- debug: var=ansible_os_family
```
#### 🔸 Q10. How do you test and debug Ansible playbooks?
Answer:

Use --check mode to do a dry run

Use --diff to see what changes will happen

Use -vvv for detailed logs

