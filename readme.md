# Ansible Playbook to deploy wordpress website on Ubuntu and Amazon Linux machines
</br>
</br>
</br>
This task requires mainly 4 YAML/YML files:

</br>

#### hosts
This is the inventory file on Ansible to assign remote machines on which ansible playbooks are applied.</br>
Tasks on ansible playbooks will be automatically executed on the remote machines mentioned in this inventory file.</br>
Both AmazonLinux and Ubuntu machine details are given under separate groups: [amazon] and [ununtu]
</br>

#### variables-amazon.yml
It contains variables required for installing apache webserver and virtualhost configuration on Amazon Linux machine.
</br>

#### variables-ubuntu.yml
It contains variables required for installing Nginx webserver configuration on Ubuntu machine.
</br>

#### wp-amazon.yml
Ansible playbook containing the following tasks:</br>
Installation of apache webserver and PHP </br>
Configuration of apache webserver from virtualhost template saved on project directory.</br>
Creation of Documentroot for domain hosted using apache webserver on Amazon Linux.</br>
Creating Sample test.html and test.php page under documentroot</br>
Installation of mariadb server and configuring mysql server installation</br>
Creation of new database user and database for Wordpress</br>
Downloading and extracting default Wordpress files from https://wordpress.org/latest.tar.gz passed from main playbook</br>
Configuring wordpress with wp-config template file from project directory </br>
Restart of services</br>
Cleanup of downloaded compressed wordpress file from https://wordpress.org/latest.tar.gz</br>
</br>

#### wp-ubuntu.yml
Ansible playbook containing the following tasks:</br>
Installation of Nginx webserver and PHP </br>
Configuration of Nginx webserver from configuration template saved on project directory.</br>
Creation of Documentroot for domain hosted using apache webserver on Ubuntu.</br>
Creating Sample test.html and test.php page under documentroot</br>
Installation of mariadb server and configuring mysql server installation</br>
Creation of new database user and database for Wordpress</br>
Downloading and extracting default Wordpress files from https://wordpress.org/latest.tar.gz passed from main playbook</br>
Configuring wordpress with wp-config template file from project directory </br>
Restart of services</br>
Cleanup of downloaded compressed wordpress file from https://wordpress.org/latest.tar.gz</br>
</br>

#### wp-include.yml
Main ansible playbook includes: </br>


Variables needed for Mariadb installation on both Ubuntu and Amazon machines under "vars" option</br>
Vars_promt option to get the database password from commandline  for wordpresss</br>
vars_files including variables-amazon.yml and variables-ubuntu.yml files</br>
tasks with above wp-amazon.yml and wp-ubuntu.yml files separately to be executed based on the conditions with ansible arguments determining Ubuntu and Amazon Linux servers</br>
</br>
</br>
To check whether there are no YAML syntax errors, use the command given below:

`ansible-playbook -i hosts wp-include.yml --syntax-check`

</br> Then execute following:

`ansible-playbook -i hosts wp-include.yml`
</br>
Result will be as follows:

```sh
Enter the password for the database user:                                                                                                             
confirm Enter the password for the database user:                                                                                                     
                                                                                                                                                      
PLAY [Playbook - Wordpress installation on AmazonLinux and Ubuntu] ***********************************************************************************
                                                                                                                                                      
TASK [Gathering Facts] *******************************************************************************************************************************
ok: [172.31.41.68]                                                                                                                                    
[WARNING]: Platform linux on host 172.31.32.244 is using the discovered Python interpreter at /usr/bin/python, but future installation of another     
Python interpreter could change this. See https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.  
ok: [172.31.32.244]                                                                                                                                   
                                                                                                                                                      
TASK [include_tasks] *********************************************************************************************************************************
skipping: [172.31.41.68]                                                                                                                              
included: /home/ec2-user/wp-amazon.yml for 172.31.32.244                                                                                              
                                                                                                                                                      
TASK [Installing Apache Webserver] *******************************************************************************************************************
changed: [172.31.32.244]                                                                                                                              
                                                                                                                                                      
TASK [Enabling PHP support] **************************************************************************************************************************
changed: [172.31.32.244]                                                                                                                              
                                                                                                                                                      
TASK [Creating httpd.conf from httpd.conf.j2 template] ***********************************************************************************************
changed: [172.31.32.244]                                                                                                                              
                                                                                                                                                      
TASK [Creating virtualhost conf from virtualhost.conf.j2 template] ***********************************************************************************
changed: [172.31.32.244]                                                                                                                              
                                                                                                                                                      
TASK [Creating Documrntroot for /var/www/html/wp-amazon.haashdev.tech] *******************************************************************************
changed: [172.31.32.244]                                                                                                                              
                                                                                                                                                      
TASK [Creating Sample test.html and test.php page under  /var/www/html/wp-amazon.haashdev.tech] ******************************************************
changed: [172.31.32.244] => (item=test.html)                                                                                                          
changed: [172.31.32.244] => (item=test.php)                                                                                                           
                                                                                                                                                      
TASK [Installing Mariadb] ****************************************************************************************************************************
changed: [172.31.32.244]                                                                                                                              
                                                                                                                                                      
TASK [ReStarting and enabling MariaDB service] *******************************************************************************************************
changed: [172.31.32.244]                                                                                                                              
                                                                                                                                                      
TASK [MariaDB -updating root password] ***************************************************************************************************************
[WARNING]: Module did not set no_log for update_password                                                                                              
changed: [172.31.32.244]                                                                                                                              
                                                                                                                                                      
TASK [Remove anonymous Users] ************************************************************************************************************************
changed: [172.31.32.244]                                                                                                                              
                                                                                                                                                      
TASK [Create a new database for wordpress] ***********************************************************************************************************
changed: [172.31.32.244]                                                                                                                              
                                                                                                                                                      
TASK [Create a new database user] ********************************************************************************************************************
changed: [172.31.32.244]                                                                                                                              
                                                                                                                                                      
TASK [Wordpress - Downloading the Wordpress installation file] ***************************************************************************************
changed: [172.31.32.244]                                                                                                                              
                                                                                                                                                      
TASK [Wordpress - Extracting the installation file] **************************************************************************************************
changed: [172.31.32.244]                                                                                                                              
                                                                                                                                                      
TASK [Wordpress - copying extracted wordpress contents from the downloaded installation file] ********************************************************
changed: [172.31.32.244]                                                                                                                              
                                                                                                                                                      
TASK [Creating wp-config.php from wp-config.php.j2] **************************************************************************************************
changed: [172.31.32.244]                                                                                                                              
                                                                                                                                                      
TASK [Post Installation - Restart] *******************************************************************************************************************
changed: [172.31.32.244] => (item=httpd)                                                                                                              
changed: [172.31.32.244] => (item=mariadb)                                                                                                            
                                                                                                                                                      
TASK [Post Installation - Cleanup] *******************************************************************************************************************
changed: [172.31.32.244] => (item=/tmp/wordpress)                                                                                                     
changed: [172.31.32.244] => (item=/tmp/wordpress.tar.gz)                                                                                              
                                                                                                                                                      
TASK [include_tasks] *********************************************************************************************************************************
skipping: [172.31.32.244]                                                                                                                             
included: /home/ec2-user/wp-ubuntu.yml for 172.31.41.68                                                                                               
                                                                                                                                                      
TASK [Installing Nginx] ******************************************************************************************************************************
changed: [172.31.41.68]                                                                                                                               
                                                                                                                                                      
TASK [nginx configuration from local templat] ********************************************************************************************************
changed: [172.31.41.68]                                                                                                                               
                                                                                                                                                      
TASK [Creating Documentroot for /var/www/html/wp-ubuntu.haashdev.tech] *******************************************************************************
changed: [172.31.41.68]                                                                                                                               
                                                                                                                                                      
TASK [Creating Sample test.html and test.php page under  /var/www/html/wp-ubuntu.haashdev.tech] ******************************************************
changed: [172.31.41.68] => (item=test.html)                                                                                                           
changed: [172.31.41.68] => (item=test.php)                                                                                                            
                                                                                                                                                      
TASK [Enabling symlink from  sites-available to sites-enabled] ***************************************************************************************
changed: [172.31.41.68]                                                                                                                               
                                                                                                                                                      
TASK [Installing Mariadb] ****************************************************************************************************************************
changed: [172.31.41.68]                                                                                                                               
                                                                                                                                                      
TASK [MariaDB -updating root password] ***************************************************************************************************************
changed: [172.31.41.68]                                                                                                                               
                                                                                                                                                      
TASK [Remove anonymous Users] ************************************************************************************************************************
ok: [172.31.41.68]                                                                                                                                    
                                                                                                                                                      
TASK [Create a new database for wordpress] ***********************************************************************************************************
changed: [172.31.41.68]                                                                                                                               
                                                                                                                                                      
TASK [Create a new database user] ********************************************************************************************************************
changed: [172.31.41.68]                                                                                                                               
                                                                                                                                                      
TASK [Wordpress - Downloading the Wordpress installation file] ***************************************************************************************
changed: [172.31.41.68]                                                                                                                               
                                                                                                                                                      
TASK [Wordpress - Extracting the installation file] **************************************************************************************************
changed: [172.31.41.68]                                                                                                                               
                                                                                                                                                      
TASK [Wordpress - copying extracted wordpress contents from the downloaded installation file] ********************************************************
changed: [172.31.41.68]                                                                                                                               
                                                                                                                                                      
TASK [Creating wp-config.php from wp-config.php.j2] **************************************************************************************************
changed: [172.31.41.68]                                                                                                                               
                                                                                                                                                      
TASK [Post Installation - Restart] *******************************************************************************************************************
changed: [172.31.41.68] => (item=nginx)                                                                                                               
changed: [172.31.41.68] => (item=mariadb)                                                                                                             
                                                                                                                                                      
TASK [Post Installation - Cleanup] *******************************************************************************************************************
changed: [172.31.41.68] => (item=/tmp/wordpress)                                                                                                      
changed: [172.31.41.68] => (item=/tmp/wordpress.tar.gz)                                                                                               
                                                                                                                                                      
PLAY RECAP *******************************************************************************************************************************************
172.31.32.244              : ok=20   changed=18   unreachable=0    failed=0    skipped=1    rescued=0    ignored=0                                    
172.31.41.68               : ok=18   changed=15   unreachable=0    failed=0    skipped=1    rescued=0    ignored=0                                    
                                                                                                                                                      
[ec2-user@ip-172-31-44-144 ~]$      
```
