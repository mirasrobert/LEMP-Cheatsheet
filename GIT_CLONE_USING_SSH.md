# Setup a ssh key to your ubuntu droplet

1. From your computer terminal, Login to your digitalocean droplet server using ssh.
  - ```bash
    # Using SSH with non-root user 
    ssh -i ~/.ssh/id_rsa <your_user>@your_droplet_ip_address
    # Non SSH login
    ssh root@your_droplet_ip_address
    ```
    
2. From your digital ocean server, Create a ssh key and name it <strong>id_rsa</strong>
  - ```bash
    ssh-keygen -t rsa
    ```
3. Go to directory of the ssh folder, and show the files inside.
  - ```bash
    cd <path to the .ssh folder>
    ll #show files inside
    ```
    
4. Copy the ssh key that you've generated
  - ```bash
    cat id_rsa.pub
    ```
    
5. Go to your github account, [SETTINGS] -> [SSH and GPG keys] , then click [New SSH Key].
6. Now go to your <strong>private</strong> repository and clone using ssh. Now you can clone github repository to your droplet.
  - ```bash
    git clone git@github.com:username/my_private_repository
    ```
    
[END]
