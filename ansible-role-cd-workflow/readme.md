# Ansible Role CD Workflow Guide
<p align="left">
  <img width="500" height="600" alt="image" src="https://github.com/user-attachments/assets/2fa29b92-fd84-4352-abed-63add4b93b66" />
</p>

---

| Created     | Last Updated | Version | Author       | Comment       | Reviewer |
|-------------|--------------|---------|--------------|----------------|----------|
| 21-07-2025  | 21-07-2025   | v1.0    | Sneha Joshi  | Internal Reviewer | Siddharth Pawar   |

---


## Table of Contents

1. [Introduction](#1-introduction)  
2. [What is the CD Workflow?](#2-what-is-the-cd-workflow)  
3. [CD Workflow Diagram](#3-cd-workflow-diagram)  
4. [CD Workflow Explanation](#4-cd-workflow-explanation)  
5. [Best Practices](#5-best-practices)  
6. [Troubleshooting](#6-troubleshooting)  
7. [Frequently Asked Questions (FAQs)](#7-frequently-asked-questions-faqs)  
8. [Conclusion](#8-conclusion)  
9. [Contacts](#9-contacts)  
10. [References](#10-references)

---

##  Introduction

This document provides a detailed overview of the Continuous Deployment (CD) workflow for Ansible roles. It describes how to automate and manage the deployment of infrastructure roles across various environments using Ansible playbooks, roles, inventories, and external tools.

---

##  What is the CD Workflow?

The Continuous Deployment workflow ensures that validated infrastructure code is automatically deployed to target environments like staging or production. It reduces manual intervention, promotes consistency, and increases release confidence.

The CD workflow typically includes:

- Environment preparation  
- Cloning or packaging the Ansible role  
- Inventory and variable setup  
- Playbook execution  
- Post-deployment validation  
- Notifications  

---

##  CD Workflow Diagram
<p align="center">
  <img width="800" height="600" alt="ansible-cd-workflow-colorful" src="https://github.com/user-attachments/assets/86d18a2e-ff23-4a47-85b1-71a40e79206b" />

</p>


---

##  CD Workflow Explanation

### Step 1: Prepare Target Environment  
Ensure the target systems are accessible via SSH and ready for deployment.

```bash
ansible all -m ping -i inventory/
````

### Step 2: Clone or Package Role

Clone the latest role version from your Git repository or Ansible Galaxy.

```bash
git clone https://github.com/your-org/ansible-role-name.git
```

### Step 3: Configure Inventory

Use a structured inventory to define your staging or production environment.

**Example: `inventory/prod/hosts.ini`**

```ini
[web]
192.168.1.10 ansible_user=ubuntu
```

### Step 4: Define Variables

Variables can be passed through `group_vars`, `host_vars`, or at runtime using `-e`.

```bash
ansible-playbook deploy.yml -i inventory/prod/ -e "env=prod version=1.0.0"
```

### Step 5: Execute Playbook

Run the playbook using the specified inventory and variables.

```bash
ansible-playbook deploy.yml -i inventory/prod/
```

### Step 6: Post-Deployment Checks

Validate the deployment by checking the status of services, application endpoints, or logs.

```bash
ansible web -a "systemctl status your-app" -i inventory/prod/
```

### Step 7: Send Notifications

Send notifications via email, Slack, or other tools after deployment is complete.

```bash
echo "Deployment Successful" | mail -s "Deployment Report" team@example.com
```

---

##  Best Practices

| No. | Practice                                                                    |
| --- | --------------------------------------------------------------------------- |
| 1   | Use separate inventories for development, staging, and production.          |
| 2   | Use version control (tags or branches) for each role.                       |
| 3   | Store sensitive data securely using Ansible Vault or external secret tools. |
| 4   | Use `--check` mode for safe validation before actual deployment.            |
| 5   | Define rollback procedures in case of failure.                              |
| 6   | Use handlers to apply changes only when necessary.                          |
| 7   | Implement proper error handling and logging.                                |

---

##  Troubleshooting

| Issue                  | Resolution                                                                  |
| ---------------------- | --------------------------------------------------------------------------- |
| SSH unreachable        | Ensure the host is up, IP is correct, and SSH keys are configured properly. |
| Inventory not loading  | Verify path and format of the inventory file.                               |
| Variables not applied  | Use `--extra-vars` or review `group_vars` and `host_vars`.                  |
| Playbook fails midway  | Use `--start-at-task` to resume from the failed step.                       |
| Role or task not found | Ensure the role path is set correctly or use `roles_path` in `ansible.cfg`. |
| Notification failed    | Confirm SMTP/Slack configuration and internet access.                       |

---

##  Frequently Asked Questions (FAQs)

#### Q1: Can I use the same role for multiple environments like dev, staging, and prod?
Yes. Structure your inventory and variables in separate folders (e.g., `inventory/dev/`, `inventory/staging/`, `inventory/prod/`) and pass environment-specific parameters through `group_vars` or `--extra-vars`.



#### Q2: What is the best way to handle secrets in deployment?
Use `ansible-vault` to encrypt secrets or integrate tools like HashiCorp Vault for external secret management. Never store plaintext secrets in your Git repository.



#### Q3: How can I roll back if something fails during deployment?
You can design rollback tasks/playbooks or use version-controlled roles where a previous version can be re-deployed. Maintain proper backups and use handlers with `when` conditions to prevent unintended changes.



#### Q4: What’s the difference between CI and CD in Ansible workflows?
CI (Continuous Integration) focuses on linting, syntax checks, and test validations without actual deployment. CD (Continuous Deployment) includes actual playbook execution and changes to the target systems.



#### Q5: Should I use `ansible-pull` or `ansible-playbook` for CD?
For most server-side orchestrated CD pipelines (like GitLab, Jenkins), use `ansible-playbook`. Use `ansible-pull` only for decentralized or agent-based deployments where each host pulls and applies changes itself.

---

##  Conclusion

A well-defined CD workflow for Ansible roles ensures efficient and reliable deployments across environments. By automating these steps, teams can eliminate manual errors, reduce deployment time, and ensure consistency in infrastructure provisioning.

---

##  Contacts

| Name        | Email                                                     |
| ----------- | --------------------------------------------------------- |
| Sneha Joshi | [sneha.joshi.snaatak@mygurukulam.co](mailto:sneha.joshi.snaatak@mygurukulam.co) |

---

##  References

| Resource              | Link                                                                                                                            |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Ansible Official Docs | [https://docs.ansible.com/](https://docs.ansible.com/)                                                                          |
| Ansible Vault         | [https://docs.ansible.com/ansible/latest/user\_guide/vault.html](https://docs.ansible.com/ansible/latest/user_guide/vault.html) |
| GitLab CD Pipelines   | [https://docs.gitlab.com/ee/ci/](https://docs.gitlab.com/ee/ci/)                                                                |                                  |
| Ansible Galaxy Roles  | [https://galaxy.ansible.com/](https://galaxy.ansible.com/)                                                                      |



