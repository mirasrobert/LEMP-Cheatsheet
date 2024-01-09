# Setup a ssh key to login using ssh 

1. From your computer terminal, Create a ssh key and name it <strong>id_server</strong> or just use default <strong>id_rsa</strong>
  - ``ssh-keygen -t rsa -b 4096 "server"``
3. Go to .ssh directory and copy the id_server.pub
  - ``cd .ssh``
  - ``ls #show all files``
  - ``cat id_server.pub | pbcopy`` or ``cat id_rsa.pub | pbcopy`` <br>
3. Login to your droplet as root user account thru ssh
  - ``ssh root@your_droplet_ip_address``
4. From your root directory of your drplet, go to .ssh folder
  - ``cd .ssh``
  - ``ll #show hidden files``
  - ``vim authorized_keys``
  - Paste the ssh key from the clipboard <i>[ctrl + v]</i>
  - Press <strong>Escape</strong>, Type <strong>:wq</strong> to save and exit. Hit Enter.
5. Exit to your droplet server, and try to login using ssh. Type:
  - ``exit``
6. Login to your droplet using the ssh key
  - ``ssh -i ~/.ssh/id_server root@your_droplet_ip_address`` OR ``ssh -i ~/.ssh/id_rsa root@your_droplet_ip_address``


# Create a Non-root user and disable root login.

1. Login to your droplet using ssh key
2. Create a new non root user
  - ``adduser <your_user>``
  - Enter also the password for the new user.
3. Granting Admin Provileges
  - ``usermod -aG sudo <your_user>`` or ``usermod -aG admin <your_user>``
4. Switch to the non-root user
 - ``sudo su <your_user>``
5. Create a .ssh folder and add a authorized_keys file
 - ``mkdir .ssh``
 - ``cd .ssh``
 - ``vim authorized_keys``
 - Paste the .ssh key of the <strong>id_server</strong> or <strong>id_rsa</strong>
 - Type ``exit``.
6. Login to your droplet using the ssh key to test if its working.
  - ``ssh -i ~/.ssh/id_server  <your_user>@your_droplet_ip_address`` or ``ssh -i ~/.ssh/id_rsa <your_user>@your_droplet_ip_address``
# Disable root login and password authentication. Only allows ssh login.
7. While you are in the droplet with the new user. Disable the root login and password authentication
  - ``sudo vim /etc/ssh/sshd_config``
  - Scroll down and find the <strong>PermitRootLogin:</strong> <strong>yes</strong>. Change <strong>yes</strong> to <strong>no</strong>
  - Scroll down and find the <strong>PasswordAuthentication:</strong> <strong>yes</strong>. Change <strong>yes</strong> to <strong>no</strong>
  - Press <strong>Escape</strong>, Type <strong>:wq</strong> to save and exit. Hit Enter.
8. Go back to root and restart the service for the changes to take effect.
  - ``sudo su root``
  - ``sudo service ssh restart``
  - ``exit``
9. Try to login using root
  - ``ssh root@your_droplet_ip_address`` and ``ssh <your_user>@your_droplet_ip_address``
  - Permission denied will show and the root login is disabled and password login is disabled.
10. Now, you can only login using ssh.
  - ``ssh -i ~/.ssh/id_server <your_user>@your_droplet_ip_address`` OR ``ssh -i ~/.ssh/id_rsa <your_user>@your_droplet_ip_address``
  - <strong>[END]</strong>


  
