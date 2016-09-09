# Udacity P5 Linux Server Configuration
---------------------------------------

1. IP Address: 52.88.221.142 SSH Port 2200

2. Application URL: http://ec2-52-88-221-142.us-west-2.compute.amazonaws.com/

3. A summary of software I installed and configuration changes made:

```
   mkdir .ssh
   touch .ssh/authorized_keys
   nano .ssh/authorized_keys
   chmod 700 .ssh
   chmod 644 .ssh/authorized_keys
   sudo cat /etc/passwd
   sudo nano /etc/ssh/sshd_config -> change port to 2200
   sudo ufw status
   sudo ufw default deny incoming
   sudo ufw default allow outgoing
   sudo ufw allow ssh
   sudo ufw allow 2200/tcp
   sudo ufw allow www
   sudo ufw allow 123
   sudo ufw enable
   sudo ufw status
   sudo service ssh restart

   sudo apt-get install git
   sudo apt-get install python-pip python-dev build-essential
   sudo apt-get install postgresql

   sudo apt-get install apache2
   sudo apt-get install libapache2-mod-wsgi

   sudo pip install virtualenv
   sudo virtualenv venv
   sudo chmod -R 777 venv
   source venv/bin/activate
   sudo pip install Flask
   sudo pip install sqlalchemy
   sudo pip install oauth2client
   deactivate

   sudo apt-get install python-psycopg2

   sudo nano  /etc/apache2/sites-available/catalog.conf
   sudo a2dissite 000-default.conf

   sudo a2ensite catalog

   sudo nano /var/www/catalog/app.wsgi
   sudo service apache2 restart

```

4. udacity_key.rsa

-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAwvFNYuy5UiY0dMMRk/N2vxdaFndbSUo+Ekw8rg9py3TU6uDn
pTGGzJW2OgoHmKPSgKwK7pvGeqaTAxXbCVxfZ4NDdtfQzpg4+dFH1ZvGX1dLvA93
z8/umIvGlHgBQxZei+oov/gPDUdSCxG/eBO4G5MEmfg3rJQZHwtC9GoE4k+PvEjM
hs1PD76IjHjxpdzwGptcNcOIkekzlU72Hx4VmQur/1meoW82gpbMOUQ2mg6+ZIe/
KOyoxq/p/JZQPSULFzMjdFQdnxPH7EgeSIBBv/vOJi7K75OctwK1pgIPXGmmaf2z
ePMkEbR4fZ1CnGHK7hvJSXPmcRDEbgGkdXzXjwIDAQABAoIBAQCByQUsh04aUzU5
R2W8iqrULkfcDTrJYJRSuk7r03cr7WyTs4iFgqOsMUBRA7xq8yaCZAgjM7LAlYXq
1/IsGUOrNskDpktuQAouyBZ7MGqxrvzNB79Mq7K9/NEIp5yvfYUUwaC/rfbXcFQ+
6mH/H6HDOe4mVAs86pyO/oBlGPCHeib6Ew9ijOWzWMFXriuHdrt49q+V6DITQag5
wT/2n3Ihnn8fqEugzw4EcQzG7fGyt6ZbLtt7aPnYvE/I2aO4a2Kkn3bSYaCIGahv
Z21NwjqktEmYyHnBBPZwBTvJ7E5n3slioG+8OoPc08UFcWy6pV+RbU5GD60hsxZg
bYOao+sBAoGBAOpkWfAgq0yUNGx+FZmz14hd47wfjvEFM3ZV/VxFORou0RY01rT7
TSp2JUJ2LEpuecYy8QyCpFxOTZ3YXKb52tP7LJ4HzDaY5wC/juik5tWTWZZD2fSH
ZDly3J3QfE3/1/kKPmTCui3Ru1w2J0T1moe2FIn2xyxDQEZ9bXbVvW3HAoGBANTp
8f9JtdDqInFjES7jdQGyGLylIg0z+aNdTbHuXOeByb3jEDlL2v1iKGbLGpgltO2R
r+lRrIcJ8xki8xYloVx420VjwDYCvE+2EKCgmRd1HLyMVCpezhaiVPY7PJO2nNdU
DI244oBkcjMhf0WVZiKClZCxmG5ptMBKHR6mXmf5AoGAOURDZRPWRmN/W3KCvIbg
BgVKetALEIAAzsy7liujg+4kT5ShUJ6Ff/ZVrCNLNvQ+9FnF4xSYK6VF+Wa/XFx3
Rot7nzCwiDZbWidzNwvzCgNyQ/BX9BKQPij+FeC2PihgEAycqemZq1AuwpzIg2Su
WLH32q1chEf6ED7c28fk/EkCgYEA0yZXipetKkyob86RofCNf2sCQUN7K6DZ9/t2
K/l6RVVfn2NqGYhy20rXSmouK6lpbxlGXZtUAHALmGgir1oOVsi8nGo6mtXHrz2d
686ZRLwuDYcViReQRr4iiDdi8hLuJFYERSCP8EitQKv9riJlsd/TODYIN6e5S+G0
U0sm4PECgYBViTuDjnFfWth2CJcURmF2O+DUoZal/6BZQauJaMhoSk0ZDX2N9oGx
kTj2RN/GkQ1cFp/wjPqYeqN8TwMu0Jd0XXBCC3r1HTdz9/xpgqGDcoy/dmWQNBNP
wsBGHmpCx+1/HanTbz00Mis4cnwHsWcCtooMiRO0z7vR4E5ammMQUw==
-----END RSA PRIVATE KEY-----

# Resources
----------
1. Configuring Linux Web Servers - Udacity course

2. https://discussions.udacity.com/c/nd004-p7-linux-based-server-configuration - Udacity Discussion Forum

