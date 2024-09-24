# Example 1

## Title: Execute Date Command and Display Output

### Overview
This playbook executes the `date` command on the localhost and displays the output in the console. It demonstrates the usage of the Ansible `command` module for executing shell commands and the `debug` module for displaying results.

### Playbook File
- **Filename**: `01_playbook_ex1.yaml`

### Inventory File
- **Filename**: `inventory.ini`
- **Purpose**: Specifies the hosts on which the playbook will run (in this case, localhost).

### Execution Command
To run this playbook, use the following command:

```bash
ansible-playbook -i inventory.ini 01_playbook_ex1.yaml --connection=local
```

### Playbook Structure

```yaml
- name: play1
  hosts: localhost
  tasks:
    - name: "execute date command"
      ansible.builtin.command: date
      register: date_output  # Store the output in that variable
    
    - name: "display the output"
      debug:
        var: date_output.stdout  # Output the date 
```

### Breakdown of Components

#### 1. Play Definition
- **Name**: `play1`
- **Hosts**: `localhost`
  - The play is designated to run on the local machine.

#### 2. Tasks

**Task 1: Execute the Date Command**
- **Name**: `execute date command`
- **Module**: `ansible.builtin.command`
  - This module is used to run shell commands on the target machine.
- **Command**: `date`
  - The `date` command retrieves the current date and time from the system.
- **Register**: `date_output`
  - The output of the command is stored in the variable `date_output`, which contains:
    - `stdout`: Standard output from the command.
    - `stderr`: Standard error from the command.
    - `rc`: Return code of the command.

**Task 2: Display the Output**
- **Name**: `display the output`
- **Module**: `debug`
  - This module is used to print information to the console.
- **Variable**: `var: date_output.stdout`
  - Displays the standard output of the `date` command executed in the previous task.

### Expected Output
Upon execution, the playbook will display the current date and time in the console, similar to the following format:

```
TASK [display the output] *****************************************************
ok: [localhost] => {
    "date_output.stdout": "Fri Sep 24 12:34:56 UTC 2024"
}
```

### Conclusion
This playbook serves as a basic example of how to execute shell commands using Ansible and display their outputs. It can be further extended to include additional commands and functionalities as needed.

