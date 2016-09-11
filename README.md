# Udacity P5 Linux Server Configuration
---------------------------------------

   1. Launch virtual machine with my Udacity Account:
      IP Address: 52.34.0.146
      Application URL: http://ec2-52-34-0-146.us-west-2.compute.amazonaws.com/

   2. Followed instructions in order to SSH into the server
    - downloaded private key
    - mv ~/Downloads/udacity_key.rsa ~/.ssh/
    - chmod 600 ~/.ssh/udacity_key.rsa
    - ssh -i ~/.ssh/udacity_key.rsa root@52.34.0.146

   3. Create user grader
    - sudo adduser grader
      put password: n1c0!gr4d3r

   4. Add grader to sudoers list
      - sudo touch /etc/sudoers.d/grader
      - sudo nano /etc/sudoers.d/grader
            grader ALL=(ALL) PASSWD:ALL
      - log into other terminal tab as root
      - switch to grader user:
            su - grader
      - test sudo commands prompt for user password
            sudo cat /etc/passwd

   5. Copy public key from root to grader
      - log with root in a tab
      - sudo cat /root/.ssh/authorized_keys
      - copy to clipboard

      - log in other tab with root then switch to grader
      - mkdir .ssh
      - touch .ssh/authorized_keys
      - nano .ssh/authorized_keys
            - paste the key
      - chmod 700 .ssh
      - chmod 644 .ssh/authorized_keys
      - test in other tab that ssh grader@52.34.0.146 -p 22 -i ~/.ssh/udacity_key.rsa works


   6. Update installed packages
      - sudo apt-get install update
      - sudo apt-get install upgrade

   7. Change port
       - sudo nano /etc/ssh/sshd_config
            - change 22 to 2200
       - sudo service ssh restart
       - logout and login with ssh grader@52.34.0.146 -p 2200 -i ~/.ssh/udacity_key.rsa

   8. Configure UFW to allow incoming connections for SSH(2200) HTTP(80) NTP(123)
        - sudo ufw status
        - sudo ufw default deny incoming
        - sudo ufw default allow outgoing
        - sudo ufw allow 2200/tcp
        - sudo ufw allow www
        - sudo ufw allow 123
        - sudo ufw enable
        - sudo ufw status

   9. Configure local timezone to UTC
        - sudo dpkg-reconfigure tzdata

   10. Disable root login
        - sudo nano /etc/ssh/sshd_config
            PermitRootLogin no

        - sudo service ssh restart
        - logout try to login with  ssh root@52.34.0.146 -p 2200 -i ~/.ssh/udacity_key.rsa

   11. Install Postgres
        - sudo apt-get install postgresql postgresql-contrib

   12. Create catalog user
        - sudo su - postgres
        - createuser catalog
        - psql
        - \password catalog
               catalog123!

   13. Create database catalog and grant access to new user
        - CREATE DATABASE catalog;
        - GRANT ALL ON DATABASE catalog TO catalog;

   14. Install and configure Apache to serve Python mod_wsgi application
       - sudo apt-get update
       - sudo apt-get install apache2
       - sudo apt-get install libapache2-mod-wsgi

   15. Clone item catalog app
       - sudo apt-get install git
       - cd /var/www
       - sudo mkdir catalog
       - sudo git clone https://github.com/orangegirl85/udacity_p3_item_catalog.git


   16. Configure app.wsgi and enable it

        - sudo touch app.wsgi
        - sudo nano app.wsgi
            ```
            #!/usr/bin/python
            import sys
            import logging
            logging.basicConfig(stream=sys.stderr)
            sys.path.insert(0,"/var/www/catalog/udacity_p3_item_catalog")
            from app import app as application
            application.secret_key = 'catalog_secret'
            ```
        - sudo a2enmod wsgi
        - sudo service apache2 reload

   17. Configure virtual host catalog

        - cd /etc/apache2/sites-available
        - sudo touch catalog.conf
        - sudo nano catalog.conf

        ```

        <VirtualHost *:80>
            ServerName 52.34.0.146
            ServerAdmin grader@52.34.0.146
            WSGIScriptAlias / /var/www/catalog/app.wsgi
            <Directory /var/www/catalog/udacity_p3_item_catalog/>
              Order allow,deny
              Allow from all
            </Directory>
            Alias /static /var/www/catalog/udacity_p3_item_catalog/app/static
            <Directory /var/www/catalog/udacity_p3_item_catalog/app/static/>
               Order allow,deny
               Allow from all
            </Directory>
            ErrorLog ${APACHE_LOG_DIR}/error.log
            LogLevel warn
            CustomLog ${APACHE_LOG_DIR}/access.log combined
        </VirtualHost>
        ```

        - sudo a2dissite 000-default.conf
        - sudo a2ensite catalog
        - sudo service apache2 reload

   18. Instal python, pip, virtualenv and install app dependencies

        - sudo apt-get install python-pip python-dev build-essential

        - sudo pip install virtualenv
        - cd /var/www/catalog
        - sudo virtualenv venv
        - sudo chmod -R 777 venv
        - source venv/bin/activate
        - sudo pip install Flask
        - sudo pip install sqlalchemy
        - sudo pip install oauth2client
        - deactivate

        - sudo apt-get install python-psycopg2

   19. Change SQLACHEMY_DATABASE_URI to 'postgresql+psycopg2://catalog:catalog123!@localhost/catalog'

        /var/www/catalog/udacity_p3_item_catalog
        sudo nano config/__init__.py

   20. Populate db
         python lotsofitems.py

   21. Add http://ec2-52-34-0-146.us-west-2.compute.amazonaws.com/ to Google, Facebook
         authorized origins in order for the login to work


udacity_key.rsa

-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEA9yrV9oFIi7KgVkU/U3j3LqWNeoDUPvpsE684k6tREdYN8KTb
92q93dkdz+Zxqkx66HLrpID3qGip8Pf+6JJ1SF+0ieW9UO9RGROe0BtHJwu8100p
ZufLYngdaIiz2XINpBfTbxi6w18z2c1dKNeRV3nZuIsqHyMoAseL2obUDG2NepY5
lZSJPoyTzIfSKrJ7gXScW1Wtldh0Sm5ypAJNxAFjMPPHssxW0NVA+8FHWWAREuu5
5BHOg7+lVUjYD8oH1TegtGlgqh3/9CoXNdIMSGBHyOkettIbH6GDU1Q9Q5ZhLxTH
DCQwnM/QTn3JeNiJEKqVEUb0jZYQzbARSwvNYQIDAQABAoIBABTCBDYvpWgWGGEm
b6sS/a9tN/SR3UFsxqbgkF/Wg3h8Aa+0KMUesdNv6JENSX6D7c6X2DJ4olQCdkNG
kKH3quHpJ8BtCvlBIA57F1ti7xbYZYOvd0qtLOeYLuAYmwIoEf02AwvRh93goPop
xSko8YvnL+HPzjnPOg0BtT0lFan1Xyu9V+7s39qVKGUl1YSb2e6tXsR7Z/2Yyl6x
7O5X38asy1ISjTB1UH4VH9I5M3paZG/C6fnUVdRi9aQx42UVCNkOxegQH7TpAKFd
UxuLjo0NkBDRUeYOe35KX47EOsA3xV91uMqBbQl11adQuqtkpDkTn8qiQ9Tm9W18
zn62IAECgYEA/UXFRILfn2Axd3V5FC7rJh4M29CVvkLUuGrS/+oEJZlrh9ex+jge
HhhJ5+s4XakrUeX4IbtGITdRhMPQity9s7kE3CBKgxI8fQLgVKyddnhBgNLdoec0
ugcJi+vy6z2HM/l8JV8MBM4hyGK8YnuVh502UZ9fioSVZExejNOfAqkCgYEA+dQ7
8wOZb3moXhfiVszUy/fnIHSx9ZprjKQ+oNqDdCQMFu+ufPeWXQcqs4N2yYXi8U7X
m0meTCYRYblORi1yYNmcfZAegRR/w/LfIh+X1igdbD7pIO7aVJhSHO6lV8UyozLY
/fHUGXDHX94QMZaYmtwE9551oAcDK43dnH0p3/kCgYACWoCfx1uOnpU3F/ddEaaL
vyG9dS2/C93wLMXzLNiHBOrz4zQ7MARPoUgmiJAIhsbpRurMXXIkYuA2DJ/GepCk
t+ZsqTwoBaZcPweYodYAwNNTACKhG4Xo6KHVFAc42mSEPiBCNKTm8odr1kcL3zwf
e32Cuqlnnx3IRdFnG5xRqQKBgAyuwIhas2xcUbbNIZlkdp0QLbSRGAOu3izana9O
yIOZ3N9BNat0aja4yWspjls2p8m2Re3FM/sLp7A9VwLrBbZy9aXOLi5BWycYakly
LyQZDz1SiEU6uuy2etyrJMuq4CI2z5s8rpbICecM/+d0jLS33KyxZ6lDEd8hKZHr
LLgJAoGAZeJ3rSQvf21XfjydNM23TNyS9cMBlR8w5mSAuArMQDoy5e0PhBMY7xeP
TOY/ueJfJvQw2l23m3uyoXyFbPrfoQLgz4anKtiBlhztHFeA5QaTq33rc7PVpCce
hJZhe/UsAglBRwEHWwKfVPsnUDXH8RbCCmyW86gPh4peY3O/F7Y=
-----END RSA PRIVATE KEY-----

# Resources
----------
1. Configuring Linux Web Servers - Udacity course

2. https://discussions.udacity.com/c/nd004-p7-linux-based-server-configuration - Udacity Discussion Forum

