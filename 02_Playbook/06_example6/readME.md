# Example 6

## Title: When Condition Illustration (Install NGINX) with AND and OR

### Overview
This playbook demonstrates how to use conditional statements with logical operators (`and` and `or`) in Ansible to manage the installation of NGINX on different operating systems. It uses the `ansible_os_family` variable to determine the OS family and the `ansible_distribution_version` variable to specify the OS version, allowing for more granular control over the installation process.

### Playbook File
- **Filename**: `06_playbook_ex6.yaml`

### Inventory File
- **Filename**: `inventory.ini`
- **Purpose**: Specifies the hosts on which the playbook will run (in this case, localhost).

### Execution Command
To run this playbook, use the following command:

```bash
ansible-playbook -i inventory.ini 06_playbook_ex6.yaml --connection=local
```

### Playbook Structure

```yaml
- name: "Install NGINX on Debian or RedHat"
  hosts: localhost
  tasks:
    - name: "Install nginx on RedHat"  # This task will be skipped if not on RedHat or SUSE
      yum: 
        name: nginx
        state: present
      when: 
        ansible_os_family == "RedHat" or
        ansible_os_family == "SUSE"
  
    - name: "Install nginx on Debian"
      apt: 
        name: nginx
        state: present
      when: 
        ansible_os_family == "Debian" and
        ansible_distribution_version == '22.04'
```

### Breakdown of Components

#### 1. Play Definition
- **Name**: `Install NGINX on Debian or RedHat`
- **Hosts**: `localhost`
  - The play is designated to run on the local machine.

#### 2. Tasks

**Task 1: Install NGINX on RedHat or SUSE**
- **Name**: `Install nginx on RedHat`
- **Module**: `yum`
  - This module is used to manage packages on RedHat-based systems.
- **Package Name**: `nginx`
  - Specifies the package to be installed.
- **State**: `present`
  - Ensures that NGINX is installed on the system.
- **Condition**: 
  - **`when: ansible_os_family == "RedHat" or ansible_os_family == "SUSE"`**
    - This condition checks if the operating system family is either RedHat or SUSE. If true, the task is executed; otherwise, it is skipped.

**Task 2: Install NGINX on Debian**
- **Name**: `Install nginx on Debian`
- **Module**: `apt`
  - This module is used to manage packages on Debian-based systems.
- **Package Name**: `nginx`
  - Specifies the package to be installed.
- **State**: `present`
  - Ensures that NGINX is installed on the system.
- **Condition**: 
  - **`when: ansible_os_family == "Debian" and ansible_distribution_version == '22.04'`**
    - This condition checks if the operating system family is Debian and if the distribution version is '22.04'. If both conditions are true, the task is executed; otherwise, it is skipped.

### Expected Output
Upon execution, the playbook will display the status of the installation process for NGINX. Depending on the OS family and version, either the task for RedHat/SUSE or Debian will execute, while the other will be skipped. You might see outputs similar to:

```
TASK [Install nginx on RedHat] ********************************************
skipping: [localhost]

TASK [Install nginx on Debian] *********************************************
changed: [localhost]
```

### Conclusion
This playbook effectively illustrates the use of conditional statements with logical operators in Ansible, allowing for dynamic task execution based on the operating system family and version. This approach enables the playbook to be versatile and applicable to different environments with specific requirements.

### Notes
- Ensure that Ansible is installed on the local machine.
- The playbook assumes that the target machine is either a RedHat, SUSE, or Debian-based OS, as determined by the `ansible_os_family` and `ansible_distribution_version` variables.
