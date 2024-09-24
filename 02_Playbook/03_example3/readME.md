
# Example 3 

## Title: Playing with Variables

### Overview
This playbook demonstrates how to use variables in Ansible to manage the installation and status of a service, specifically Apache2 (or any other service defined by the variable). It ensures that the service is installed, verifies its installation status, and checks if the service is running.

### Playbook File
- **Filename**: `03_playbook_ex3.yaml`

### Inventory File
- **Filename**: `inventory.ini`
- **Purpose**: Specifies the hosts on which the playbook will run (in this case, localhost).

### Execution Command
To run this playbook, use the following command:

```bash
ansible-playbook -i inventory.ini 03_playbook_ex3.yaml --connection=local
```

### Playbook Structure

```yaml
- name: play1
  hosts: localhost
  vars:
    server_name: apache2
  tasks:
    - name: "Ensure that {{server_name}} is installed and up-to-date"
      ansible.builtin.apt:  # sudo apt-install server_name 
        name: "{{server_name}}"
        state: latest
      register: installed  # Store the output in that variable
    
    - name: "Display {{server_name}} installation status"
      debug:
        # Display if apt install server_name causes any change in your computer 
        # or not (change = an update or install)
        var: installed.changed 
    
    - name: "Check if {{server_name}} service is running"
      ansible.builtin.service: 
        name: "{{server_name}}"
        state: started
      register: service_status
    
    - name: "Display {{server_name}} service status"
      debug:
        var: service_status.state  # Display the status of the service
```

### Breakdown of Components

#### 1. Play Definition
- **Name**: `play1`
- **Hosts**: `localhost`
  - The play is designated to run on the local machine.

#### 2. Variables
- **`vars`**: 
  - **`server_name: apache2`**: This variable holds the name of the service to be managed. It can be modified to manage different services without changing the tasks.

#### 3. Tasks

**Task 1: Ensure the Service is Installed and Up-to-Date**
- **Name**: `Ensure that {{server_name}} is installed and up-to-date`
- **Module**: `ansible.builtin.apt`
  - This module is used to manage packages on Debian-based systems (like Ubuntu).
- **Package Name**: `{{server_name}}`
  - Uses the variable `server_name` to specify the package to be installed or updated.
- **State**: `latest`
  - Ensures that the latest version of the specified service is installed.
- **Register**: `installed`
  - Stores the output of the installation process, which includes whether any changes were made.

**Task 2: Display Installation Status**
- **Name**: `Display {{server_name}} installation status`
- **Module**: `debug`
  - This module is used to print information to the console.
- **Variable**: `var: installed.changed`
  - Displays whether the installation or update of the service caused any changes (i.e., if it was newly installed or updated).

**Task 3: Check if the Service is Running**
- **Name**: `Check if {{server_name}} service is running`
- **Module**: `ansible.builtin.service`
  - This module is used to manage services on the target machine.
- **Service Name**: `{{server_name}}`
  - Uses the variable `server_name` to manage the specified service.
- **State**: `started`
  - Ensures that the specified service is running.
- **Register**: `service_status`
  - Stores the output of the service status check, including the current state of the service.

**Task 4: Display Service Status**
- **Name**: `Display {{server_name}} service status`
- **Module**: `debug`
  - This module is used to print information to the console.
- **Variable**: `var: service_status.state`
  - Displays the current state of the service (e.g., `running`, `stopped`).

### Expected Output
Upon execution, the playbook will display the status of the service installation and its running state. You might see outputs similar to:

```
TASK [Display {{server_name}} installation status] ******************************
ok: [localhost] => {
    "installed.changed": true  # If the service was installed or updated
}

TASK [Display {{server_name}} service status] ************************************
ok: [localhost] => {
    "service_status.state": "running"  # If the service is running
}
```

### Conclusion
This playbook illustrates how to effectively use variables in Ansible to manage service installations and their states. By using the `server_name` variable, the playbook becomes more flexible and can easily be adapted to manage different services.

### Notes
- Ensure that Ansible is installed on the local machine.
- The playbook assumes that the target machine uses a Debian-based OS (like Ubuntu) due to the use of the `apt` module.
