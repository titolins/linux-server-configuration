# Udacity's Full Stack Web Developer Nanodegree
## Project 5
### Linux Server Configuration

date: 2016-02-23

author: Tito Lins

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
    rsa key contents to ~/.ssh/udacity_grader.rsa, issue the following command
    for ssh access:
    ```
    ssh -i ~/.ssh/udacity_grader.rsa grader@52.37.32.202 -p 2200
    ```

* Website access:
    * To access the application through your web browser, use the following
    address:
    ```
    http://ec2-52-37-32-202.us-west-2.compute.amazonaws.com/
    ```

* Packages installed
    * after upgrading the whole system, the following additional packages were
    installed:
    ```
    apache2
    libapache2-mod-wsgi
    ```

* Configurations
    * User
        * We have created the grader user, as required by the project, and
        chose to use it as the default user. As such, it has been included in
        the sudoers file (it's password has been provided together with the
        private rsa key).
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

#### Third Party Resources
#TODO
