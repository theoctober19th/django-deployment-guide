# Deploying Django application using Apache Server and mod_wsgi using Raspberry Pi

1. Make sure everything is up to date.  
    `sudo apt-get update -y && sudo apt-get upgrade -y`

2. Install `apache2` server.  
    `sudo apt-get install apache2 -y`

3. Install `mod_wsgi` using command:  
    `sudo apt-get install libapache2-mod-wsgi`

4. Navigate to Apache conf directory:  
    `cd /etc/apache2/sites-available/`

5. Create a new configuration file with command:  
    `sudo nano django_config.conf`

6. The contents for `django_config.conf` file just created are [here](./django_config.conf). Make sure to change the names `project_name`.

7. The django project should be cloned/installed/created at `/var/www/`

8. Make sure that the directory `/var/www/logs/` exists. Create it if it doesn't exist.

9. Activate `django_config.conf`  
    `sudo a2ensite django_config.conf`

10. Restart Apache Server  
    `sudo /etc/init.d/apache2 restart`

11. Now edit the Apache configuration file with:  
    `sudo nano /etc/apache2/apache2.conf`

12. Add [these lines](./apache2.conf) at the end of the configuration file. Make sure to change the names `project_name`.  

13. Restart Apache Server  
    `sudo /etc/init.d/apache2 restart`

14. Change permissions to `/var/www/project_name`, database file as well as other directories that should be written. (By default they are read only)

Voila! Access IP address of Raspberry Pi from any device in the network and you should see the application running on top of Apache server.