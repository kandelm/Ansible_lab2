# Ansible_lab2
1- If we use the following inventory, on which hosts will Ansible install the httpd package using the given playbook?


[webserver]
web1
web2
[appserver]
app1
app2
app3

---
- name: Setup apache
  hosts: webserver
  tasks:
    - name: install httpd
      yum:
        name: httpd
        state: installed

- name: Setup tomcat
  hosts: appserver
  tasks:
    - name: install httpd
      yum:
        name: tomcat
        state: installed

The answer :
    In the given playbook, Ansible will install the `httpd` package only on the hosts listed under the `webserver` group, which are:

- `web1`
- `web2`

The second part of the playbook installs the `tomcat` package (not `httpd`) on the hosts listed under the `appserver` group, which are:

- `app1`
- `app2`
- `app3`

So, the `httpd` package will only be installed on `web1` and `web2`.


2- what commands can you use to run an Ansible playbook named install.yaml?

    ansible-playbook install.yaml

3-what command would you use to run update_service.yml playbook in check mode?

    ansible-playbook --check update_service.yml

    4- When you run the `update_service.yml` playbook in check mode, the tasks will show what changes would be made without applying them. The behavior of each task in check mode is as follows:

    - Install a new package : This task will check if the package `new_package` is installed. If it is not installed, this task will show that it would result in a change (i.e., the package would be installed). If the package is already installed, it will show no changes.

    - **Update the service: This task will check if the service `my_service` needs to be restarted. In check mode, it will report that a restart would be performed if the service is not already in the desired state.

    - Check service status: This task only verifies the status of the service, and it won't make any changes. It will show that the service is already running or not, but it will not result in any change status.

    So, if the `new_package` is not already installed, and `my_service` is not in the desired state, the tasks that would result in a changed status in check mode are:

    - A. Install a new package (if `new_package` is not installed)
    - B. Update the service (if `my_service` is not already in the desired state)

    Therefore, the correct answer would be:

    A and B.


4- Consider again the same sample playbook named update_service.yml as shown below.
    Let's suppose you have already ran this playbook on your server. Now, once you run this playbook in check mode against same server, which tasks would result in changed status?

          A. Install a new package
          B. Update the service
          C. Check service status
          D. All of the tasks
        Answer:  Update the service.

5- There is another sample playbook named configure_database.yml that modifies a configuration file on your database servers. The initial code sample is as follows:
    - hosts: all
      tasks:
        - name: Set max connections
          lineinfile:
            path: /etc/postgresql/12/main/postgresql.conf
            line: 'max_connections = 500'

        - name: Set listen addresses
          lineinfile:
            path: /etc/postgresql/12/main/postgresql.conf
            line: 'listen_addresses = "*"'

what command would you use to run the configure_database.yml playbook in both check mode and diff mode?

Answer: ansible-playbook configure_database.yml --check --diff

6- Consider again the same sample playbook named configure_database.yml as shown below \
	what is the command to check syntax error

what command would you use to run ansible-lint on the database_setup.yml playbook?

Answer:
    Command to check for syntax errors in the configure_database.yml playbook:
        ansible-playbook configure_database.yml --syntax-check
    Command to run ansible-lint on the database_setup.yml playbook:
        ansible-lint database_setup.yml
    

7- Consider again the same sample playbook named database_setup.yml as shown below.

      ```bash
      - name: Database Setup Playbook
        hosts: db_servers
        tasks:
          - name: Ensure PostgreSQL is installed
            apt:
              name: postgresql
              state: latest
              update_cache: yes

          - name: Start PostgreSQL service
            service:
              name: postgresql
              state: started

          - copy:
              src: /path/to/pg_hba.conf
              dest: /etc/postgresql/12/main/pg_hba.conf
            notify:
              - Restart PostgreSQL
      ```
After running ansible-lint on the playbook, which of the following issues might you expect to see [make sure to select two choice]?

    A. Incorrect indentation.
    B. Deprecated 'apt' module.
    C. Missing 'name' attribute for a task.
    D. Use of a blacklisted command.


Answer : After running `ansible-lint` on the provided playbook, the most likely issues you'd expect to see are:

A. Incorrect indentation.
Ansible linting tools are sensitive to YAML indentation errors, and this could be a common issue if not properly formatted.

C. Missing 'name' attribute for a task.  
The `copy` task in the playbook is missing a `name` attribute, which is a best practice in Ansible playbooks. This could trigger a warning from `ansible-lint`.

The other two options—`B. Deprecated 'apt' module` and `D. Use of a blacklisted command`—are less likely because the `apt` module is not deprecated, and there is no indication of a blacklisted command being used in the playbook.

8- You've been given feedback from ansible-lint about potential issues in your hypothetical webserver_setup.yml playbook. The feedback mentions issues with indentation, deprecated modules, and missing name attributes.
Which of the following is NOT a recommended action based on the feedback?

    A. Correcting the indentation in the playbook.
    B. Replacing deprecated modules with their newer counterparts.
    C. Ignoring the feedback and proceeding with playbook execution.
    D. Adding 'name' attributes to tasks that are missing them.

The correct answer is:
Ignoring the feedback and proceeding with playbook execution.

Ignoring the feedback is not a recommended action. Addressing feedback from tools like `ansible-lint` is crucial for maintaining best practices, ensuring playbook reliability, and avoiding potential issues during execution.


9- If ansible-lint provides no output after checking a playbook, what does it indicate?

A. The playbook has syntax errors.
B. The playbook is empty.
C. The playbook adheres to best practices and has no style-related issues.
D. ansible-lint failed to check the playbook
 
 
The correct answer is:

C.The playbook adheres to best practices and has no style-related issues.
If `ansible-lint` provides no output, it indicates that the playbook has passed all checks and adheres to best practices, with no style-related issues detected.

10-Solve issues in below playbook

```bash
---
- hosts: localhost
  become: yes
  tasks:
    - name: 'Execute a date command on localhost'
      command: date

```

The correct answer is:

C. The playbook adheres to best practices and has no style-related issues.

If `ansible-lint` provides no output, it indicates that the playbook has passed all checks and adheres to best practices, with no style-related issues detected.

11-Create an Ansible playbook to install Nginx on a group of servers. Ensure the service is started and enabled at boot.

then run this command:
    ansible-playbook install_nginx.yml
