# Deploy-Application-on-tomcat-docker-container-using-Ansible

To deploy an application on a Tomcat Docker container using Ansible, you can follow a similar approach to deploying applications on a traditional server. However, instead of installing Tomcat directly on a host machine, you'll create a Docker container running Tomcat and deploy the application into that container. Below is a step-by-step guide to create an Ansible project for deploying an application on a Tomcat Docker container.

### Summary

- **Project Structure**: Organize your project with inventories, playbooks, and roles.
- **Ansible Playbooks**: Define tasks to deploy the application into a Tomcat Docker container.
- **Roles**: Use roles to encapsulate tasks for application deployment.
- **Docker Connection**: Use `ansible_connection=docker` in the inventory file to tell Ansible to communicate with Docker containers.


### Project Structure

Create the following directory structure for your Ansible project:

```
ansible-tomcat-docker/
├── inventories/
│   └── production/
│       └── hosts.ini         # Inventory file
├── playbooks/
│   └── deploy-app.yml        # Playbook to deploy the application
└── roles/
    └── deploy-app/
        ├── tasks/
        │   └── main.yml      # Deploy application tasks
        ├── files/
        │   └── sample.war    # Sample application WAR file (replace with your app)
        └── meta/
            └── main.yml      # Deploy application metadata
```

### Step-by-Step Guide

#### 1. Inventory File

Create an inventory file (`hosts.ini`) under `inventories/production/`:

```ini
[tomcat_containers]
localhost ansible_connection=docker
```

In this inventory, we define a group `tomcat_containers` with `localhost` as the only member. We specify `ansible_connection=docker` to tell Ansible to communicate with Docker containers instead of SSHing into remote hosts.

#### 2. Deploy Application Playbook

Create a playbook (`deploy-app.yml`) under `playbooks/` to deploy the application to a Tomcat Docker container:

```yaml
---
- name: Deploy application to Tomcat Docker container
  hosts: tomcat_containers
  become: true  # Run tasks with root privileges

  roles:
    - deploy-app
```

#### 3. Deploy Application Role

Create an Ansible role for deploying the application under `roles/deploy-app/`:

**`roles/deploy-app/meta/main.yml`**:

```yaml
---
dependencies: []
```

**`roles/deploy-app/tasks/main.yml`**:

```yaml
---
- name: Copy WAR file to Tomcat webapps directory
  copy:
    src: files/sample.war  # Replace with your WAR file
    dest: /usr/local/tomcat/webapps/sample.war
    owner: root
    group: root
    mode: '0644'
```

Make sure to replace `files/sample.war` with the path to your actual WAR file that you want to deploy.

#### 4. Running the Playbook

To deploy the application into a Tomcat Docker container, run the following command:

```bash
ansible-playbook -i inventories/production/hosts.ini playbooks/deploy-app.yml
```


This setup provides a scalable approach to deploying applications on Tomcat Docker containers using Ansible. Customize the configurations and tasks as needed for your application and environment requirements.
