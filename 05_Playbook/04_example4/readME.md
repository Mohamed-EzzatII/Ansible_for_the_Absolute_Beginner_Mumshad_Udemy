# Example 4 

## Title: Playing with Variables Part II

### Overview
This playbook demonstrates how to use variables, specifically lists, in Ansible to manage the installation and status of multiple services. In this case, it focuses on ensuring that the Apache2 service is installed and running.

### Playbook File
- **Filename**: `04_playbook_ex4.yaml`

### Inventory File
- **Filename**: `inventory.ini`
- **Purpose**: Specifies the hosts on which the playbook will run (in this case, localhost).

### Execution Command
To run this playbook, use the following command:

```bash
ansible-playbook -i inventory.ini 04_playbook_ex4.yaml --connection=local
```

### Playbook Structure

```yaml
- name: play1
  hosts: localhost
  vars:
    servers:
      - nginx
      - apache2
  tasks:
    - name: "Ensure that {{servers[1]}} is installed and up-to-date"
      ansible.builtin.apt:  # sudo apt-install servers[1] 
          name: "{{servers[1]}}"
          state: latest
      register: installed  # Store the output in that variable
    
    - name: "Display {{servers[1]}} installation status"
      debug:
        # Display if apt install servers[1] causes any change in your computer 
        # or not (change = an update or install)
        var: installed.changed 
    
    - name: "Check if {{servers[1]}} service is running"
      ansible.builtin.service: 
        name: "{{servers[1]}}"
        state: started
      register: service_status
    
    - name: "Display {{servers[1]}} service status"
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
  - **`servers`**: 
    - A list of service names that can be managed by the playbook.
    - In this case, it contains two services: `nginx` and `apache2`.

#### 3. Tasks

**Task 1: Ensure the Service is Installed and Up-to-Date**
- **Name**: `Ensure that {{servers[1]}} is installed and up-to-date`
- **Module**: `ansible.builtin.apt`
  - This module is used to manage packages on Debian-based systems (like Ubuntu).
- **Package Name**: `{{servers[1]}}`
  - Uses the second element of the `servers` list (i.e., `apache2`) to specify the package to be installed or updated.
- **State**: `latest`
  - Ensures that the latest version of the specified service is installed.
- **Register**: `installed`
  - Stores the output of the installation process, which includes whether any changes were made.

**Task 2: Display Installation Status**
- **Name**: `Display {{servers[1]}} installation status`
- **Module**: `debug`
  - This module is used to print information to the console.
- **Variable**: `var: installed.changed`
  - Displays whether the installation or update of the service caused any changes (i.e., if it was newly installed or updated).

**Task 3: Check if the Service is Running**
- **Name**: `Check if {{servers[1]}} service is running`
- **Module**: `ansible.builtin.service`
  - This module is used to manage services on the target machine.
- **Service Name**: `{{servers[1]}}`
  - Uses the second element of the `servers` list (i.e., `apache2`) to manage the specified service.
- **State**: `started`
  - Ensures that the specified service is running.
- **Register**: `service_status`
  - Stores the output of the service status check, including the current state of the service.

**Task 4: Display Service Status**
- **Name**: `Display {{servers[1]}} service status`
- **Module**: `debug`
  - This module is used to print information to the console.
- **Variable**: `var: service_status.state`
  - Displays the current state of the service (e.g., `running`, `stopped`).

### Expected Output
Upon execution, the playbook will display the status of the Apache2 installation and its running state. You might see outputs similar to:

```
TASK [Display {{servers[1]}} installation status] ******************************
ok: [localhost] => {
    "installed.changed": true  # If Apache2 was installed or updated
}

TASK [Display {{servers[1]}} service status] ************************************
ok: [localhost] => {
    "service_status.state": "running"  # If the service is running
}
```

### Conclusion
This playbook illustrates how to effectively use lists as variables in Ansible to manage service installations and their states. By referencing elements of the `servers` list, the playbook becomes flexible and can be easily modified to manage different services by changing the indices.

### Notes
- Ensure that Ansible is installed on the local machine.
- The playbook assumes that the target machine uses a Debian-based OS (like Ubuntu) due to the use of the `apt` module.
