
# Ansible Inventory File

This document describes the structure and contents of an Ansible inventory file used for managing servers in a hypothetical course. This inventory file specifies hosts, connection types, and groups for organizing server resources.

## Overview

An Ansible inventory file is a key component for automation with Ansible. It defines the hosts that Ansible can manage and how to connect to them. This example inventory file demonstrates how to configure both local and remote hosts for a web server and a database server setup.

## Inventory Structure

### 1. Localhost Entry

```ini
localhost ansible_connection=local
```
- **localhost**: This entry defines the local machine where Ansible will execute tasks directly.
- **ansible_connection=local**: This indicates that Ansible should run tasks on the local machine without needing SSH.

### 2. Remote Web Servers

```ini
web1 ansible_host=server1.company.com ansible_user=web1 ansible_password=123456 ansible_port=22 ansible_connection=ssh
web2 ansible_host=server2.company.com ansible_user=web2 ansible_password=123456 ansible_port=22 ansible_connection=ssh
```
- **web1** and **web2**: These are the identifiers for the web server hosts.
- **ansible_host**: The domain name or IP address of the remote host.
- **ansible_user**: The username used to connect to the host via SSH.
- **ansible_password**: The password for the specified user (Note: storing passwords in plain text is not recommended; consider using Ansible Vault).
- **ansible_port**: The port used for SSH connections (default is 22).
- **ansible_connection=ssh**: This specifies that Ansible should connect to the host using SSH.

### 3. Remote Database Servers

```ini
db1 ansible_host=server1.company.com ansible_user=db1 ansible_password=123456 ansible_port=3389 ansible_connection=winrm
db2 ansible_host=server2.company.com ansible_user=db2 ansible_password=123456 ansible_port=3389 ansible_connection=winrm
```
- **db1** and **db2**: Identifiers for the database server hosts.
- **ansible_host**: Similar to web servers, this indicates the database host's domain name or IP.
- **ansible_user**: The username for the database connection.
- **ansible_password**: The password for the database user.
- **ansible_port**: The port for WinRM (default is 3389 for Windows).
- **ansible_connection=winrm**: This indicates that Ansible should connect using the Windows Remote Management (WinRM) protocol.

### 4. Grouping Hosts

```ini
[web_servers]
web1
web2

[database_servers]
db1
db2
```
- **[web_servers]**: A group that includes all web server hosts.
- **[database_servers]**: A group for the database server hosts.

### 5. Parent Group

```ini
[all_servers:children]
web_servers
database_servers
```
- **[all_servers:children]**: This defines a parent group called `all_servers`, which includes the `web_servers` and `database_servers` groups.
- This hierarchical organization allows for easier management of tasks and playbooks across different types of servers.

## Conclusion

This inventory file provides a clear structure for managing a mixed environment of web and database servers using Ansible. It allows users to run tasks across multiple hosts simultaneously and facilitates efficient automation. Remember to secure sensitive data like passwords, ideally using Ansible Vault or external credential management systems.
