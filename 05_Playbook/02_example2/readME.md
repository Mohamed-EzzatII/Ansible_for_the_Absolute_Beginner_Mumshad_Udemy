# Example 2

## Title: Check Status of Apache2 Service

### Overview
This playbook ensures that the Apache2 web server is installed and running on the localhost. It checks for the latest version of Apache2, confirms the installation status, and verifies whether the Apache2 service is active.

### Playbook File
- **Filename**: `02_playbook_ex2.yaml`

### Inventory File
- **Filename**: `inventory.ini`
- **Purpose**: Specifies the hosts on which the playbook will run (in this case, localhost).

### Execution Command
To run this playbook, use the following command:

```bash
ansible-playbook -i inventory.ini 02_playbook_ex2.yaml --connection=local
```

### Playbook Structure

```yaml
- name: play1
  hosts: localhost
  tasks:
    - name: "Ensure that apache2 is installed and up-to-date"
      ansible.builtin.apt:
        name: apache2
        state: latest
      register: installed  # Store the output in that variable
    
    - name: "Display apache2 installation status"
      debug:
        var: installed.changed  # Display if the installation caused any changes
    
    - name: "Check if apache2 service is running"
      ansible.builtin.service:
        name: apache2
        state: started
      register: apache_service_status
    
    - name: "Display apache2 service status"
      debug:
        var: apache_service_status.state  # Display the status of apache2
```

### Breakdown of Components

#### 1. Play Definition
- **Name**: `play1`
- **Hosts**: `localhost`
  - The play is designated to run on the local machine.

#### 2. Tasks

**Task 1: Ensure Apache2 is Installed and Up-to-Date**
- **Name**: `Ensure that apache2 is installed and up-to-date`
- **Module**: `ansible.builtin.apt`
  - This module is used to manage packages on Debian-based systems (like Ubuntu).
- **Package Name**: `apache2`
  - This specifies the package to be installed or updated.
- **State**: `latest`
  - Ensures that the latest version of Apache2 is installed.
- **Register**: `installed`
  - Stores the output of the installation process, which includes whether any changes were made.

**Task 2: Display Apache2 Installation Status**
- **Name**: `Display apache2 installation status`
- **Module**: `debug`
  - This module is used to print information to the console.
- **Variable**: `var: installed.changed`
  - Displays whether the installation or update of Apache2 caused any changes (i.e., if it was newly installed or updated).

**Task 3: Check if Apache2 Service is Running**
- **Name**: `Check if apache2 service is running`
- **Module**: `ansible.builtin.service`
  - This module is used to manage services on the target machine.
- **Service Name**: `apache2`
  - The name of the service to be managed.
- **State**: `started`
  - Ensures that the Apache2 service is running.
- **Register**: `apache_service_status`
  - Stores the output of the service status check, including the current state of the service.

**Task 4: Display Apache2 Service Status**
- **Name**: `Display apache2 service status`
- **Module**: `debug`
  - This module is used to print information to the console.
- **Variable**: `var: apache_service_status.state`
  - Displays the current state of the Apache2 service (e.g., `running`, `stopped`).

### Expected Output
Upon execution, the playbook will display the status of the Apache2 installation and service. You might see outputs similar to:

```
TASK [Display apache2 installation status] ************************************
ok: [localhost] => {
    "installed.changed": true  # If Apache2 was installed or updated
}

TASK [Display apache2 service status] *****************************************
ok: [localhost] => {
    "apache_service_status.state": "running"  # If the service is running
}
```

### Conclusion
This playbook provides a straightforward way to ensure that Apache2 is installed and running on a local machine. It can be easily extended to include additional configurations or checks as needed.

### Notes
- Ensure that Ansible is installed on the local machine.
- The playbook assumes that the target machine uses a Debian-based OS (like Ubuntu) due to the use of the `apt` module.