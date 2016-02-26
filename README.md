# Udacity's Full Stack Web Developer Nanodegree
## Project 5
### Linux Server Configuration

```
date: 2016-02-26
author: Tito Lins
```

#### Description
This is the last project required for the completion of the Full Stack Web
Developer Nanodegree.

In this project, we were required to configure a web server to run the
application developed during the 3rd project of this course.

#### Server info
* SSH Access:
    * The server ip address is 52.37.32.202 and the current port open for ssh
      access is 2200. To be able to access, you need the provided private rsa
      key, as password access is disabled. Considering that you have saved the
      rsa key contents provided in the notes to reviewer section to
      `~/.ssh/udacity_key.rsa`, issue the following command for ssh access:
    ```
    ssh -i ~/.ssh/udacity_key.rsa grader@52.37.32.202 -p 2200
    ```
    * Note that if the permissions on the private key are to open, such key
      will be ignored and you will not be able to access the server. To correct
      that, simply change the permissions of the private key file, as below:
    ```
    sudo chmod 600 ~/.ssh/udacity_key.rsa
    ```

* Website access:
    * To access the application through your web browser, use the following
      address:
    ```
    http://ec2-52-37-32-202.us-west-2.compute.amazonaws.com/
    ```

* Packages installed
    * after upgrading the whole system, the following additional packages were
      installed (packages installed by apt-get as dependecies of the packages
      listed below were not included in this listing):
    ```
    apache2
    libapache2-mod-wsgi
    postgresql
    postgresql-server-dev-all
    git
    python-pip
    libpython-dev
    libffi-dev
    libssl-dev
    ntp
    fail2ban
    sendmail
    iptables-persistent
    unattended-upgrades
    logwatch
    ```

* Configuration
    * User
        * We have created the grader user, as required by the project, and
          and have included him the sudoers file (it's password has been
          provided together with the private rsa key).
    * SSH
        * We have changed the ssh port to 2200, and blocked root login.
    * Firewall
        * We have configured ufw to allow all outgoing requests and deny all
          incoming requests by default. The following exceptions for inbound
          requests have been configured.
        ```
        2200/tcp
        80/tcp
        123
        ```
    * Clock / NTP
        * The clock has been set to UTC, as per the below:
        ```
        dpkg-reconfigure tzdata
        ```
        * NTP configuration was as easy as installing it (using default ubuntu
          servers).
    * Apache
        * Apache has been configured to serve a python wsgi application, as per
          the configuration file below:
        ```
        /etc/apache2/sites-available/catalog_app.conf
        ```
    * PostgreSQL
        * Created psql and respective unix user catalog with login disabled.
        * Created catalog_app db and granted all privileges to it (later to be
          restricted).
        * There was no need to disable remote connections, as this is the
          current default.
    * Catalog App
        * Cloned git repository into:
        ```
        /var/www/catalog_app
        ```
        * Created the secret key file at the instance folder (taken from [1]):
        ```
        mkdir -p /var/www/catalog_app/instance
        head -c 24 /dev/urandom > /var/www/catalog/instance/secret_key
        ```
        * Created log folder:
        ```
        mkdir -p /var/www/catalog_app/log
        ```
        * Updated redirect and origin uri's at oauth providers, and changed
          client_secrets.json (which contains google oauth configuration).
        * Set ownership to default apache user (www-data), and applied the
          following permissions:
        ```
        directories -> 750
        files -> 640
        ```
        * Created `/var/www/catalog_app/catalog_app.wsgi`, with the contents
          below (taken from [2]):
        ```
        import sys, os
        sys.path.append('/var/www/catalog_app')
        os.chdir('/var/www/catalog_app')
        from catalog_app import app as application
        ```
        * Installed required python packages, as per the application README:
        ```
        cd /var/www/catalog_app/
        pip install -r requirements.txt
        ```
    * Fail2Ban
        * Fail2Ban was installed in order to protect the server against
          repeated ssh login attempts.
        * The configuration file is located at `/etc/fail2ban/jail.local`.
    * Glances
        * Glances was installed for system monitoring.
    * unattended-upgrades
        * Installed to manage automated security updates only.
        * Configurations are located at: `/etc/apt/apt.conf.d/`
    * Logwatch
        * Logwatch was installed for log monitoring and reporting.
        * Configuration at: `/usr/share/logwatch/default.conf/logwatch.conf`
        * Respective cron job set to run daily.


#### Third Party Resources
* http://www.postgresql.org/docs/9.0/static/sql-grant.html
* http://www.postgresql.org/docs/9.0/static/sql-alterrole.html
* http://flask.pocoo.org/docs/0.10/deploying/mod_wsgi/
* https://www.digitalocean.com/community/tutorials/how-to-use-roles-and-manage-grant-permissions-in-postgresql-on-a-vps--2
* http://damyanon.net/flask-series-logging/
* http://stackoverflow.com/questions/14037975/how-do-i-write-flasks-excellent-debug-log-message-to-a-file-in-production
* https://gist.github.com/ibeex/3257877
* http://flask.pocoo.org/docs/0.10/errorhandling/
* http://docs.sqlalchemy.org/en/latest/core/engines.html#postgresql
* https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
* http://askubuntu.com/questions/117359/how-do-i-change-the-timezone-to-utc
* https://www.digitalocean.com/community/tutorials/how-to-protect-ssh-with-fail2ban-on-ubuntu-14-04
* http://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers
* https://www.digitalocean.com/community/tutorials/how-to-install-and-use-logwatch-log-analyzer-and-reporter-on-a-vps

##### References
* [1] http://flask.pocoo.org/snippets/104/
* [2] http://flask.pocoo.org/snippets/99/
