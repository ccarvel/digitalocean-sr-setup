## DIGITALOCEAN INITIAL SERVER SETUP (UBUNTU 22.04 with DOCKER)
### USING DIGITALOCEAN, INSTALL DOCKER-ONE-CLICK + UBUNTU 22.04 SERVER FROM DO MARKETPLACE
***This will give you an Ubuntu Server with a root user AND Docker that runs on startup***
*In the DigitalOcean dashboard, your droplet will be built and boot; a public IP address will be assigned as well*

***If you aren’t using DigitalOcean, and developing locally, install Docker for your OS***
[GET DOCKER](https://docs.docker.com/get-docker/)

***Commands you will need to use (and should have on your OS by default)***   
```console
openssh   
rsync   
ufw   
git   
gnupg (gpg)   
nano    
```   
***…but if you get a ```command not found``` and are running Ubuntu or Debian, you can install them***   
```console sudo apt install openssl-client```   
```console sudo apt install rsync```   
```console sudo apt install ufw```   
```console sudo apt install gnupg```   
```console sudo apt install nano```   
***other commands to be installed will be in the instructions as needed***  

### Create an SSH KEY on your local computer first
***To Connect without a password, create an SSH KEY on your local computer first***
[See Here](https://docs.digitalocean.com/products/droplets/how-to/add-ssh-keys/create-with-openssh/)
*eg* on macOS or Linux
```console
cd ~/.ssh
```
```console
ssh-keygen
```
*give your key a reasonably memorable name*
```id_oceanubu```
*It's probably a good idea to give your key a password, but I usually don't--if you do, make sure you store it safely in a password manger; Once you've finished creating your key you'll get a message that looks like:*

```console
Your identification has been saved in /home/username/.ssh/id_oceanubu.
Your public key has been saved in /home/username/.ssh/id_oceanubu.pub.
The key fingerprint is:
a9:49:EX:AM:PL:E3:3e:a9:de:4e:77:11:58:b6:90:26 username@ip.address.x.x.x
The key's randomart image is:
+--[ RSA 2048]----+
|     ..o         |
|   E o= .        |
|    o. o         |
|        ..       |
|      ..S        |
|     o o.        |
|   =o.+.         |
|. =++..          |
|o=++.            |
+-----------------+
```
### ADD SSH-KEY VIA WEB PORTAL
[Add your public key to your DigitalOcean account](https://docs.digitalocean.com/products/droplets/how-to/add-ssh-keys/to-team/) to be able to embed it in new Droplets on creation.
[Add your public key to existing Droplets](https://docs.digitalocean.com/products/droplets/how-to/add-ssh-keys/to-existing-droplet/) to use SSH key authentication to log in to them.

### CONNECT VIA SSH AS ROOT 

```console
ssh -i ~/.ssh/id_oceanubu root@ip.address.x.x.x
```

***Once connected and logged in:***
### UPDATE/UPGRADE WITH APT
```console
sudo apt update && sudo apt upgrade -y
```

### CHECK THAT DOCKER IS READY
```console
docker run hello-world
```
### ADD A USER

```console
adduser newuser
```

***(ENTER PASSWORD, USER DETAILS, 2X)***

### ADD USER TO SUDO GROUP

```console
usermod -aG sudo newuser
```

```console
usermod -aG docker newuser
```

### RSYNC ROOT .SSH DIR TO NEW USER

```console
rsync --archive --chown=newuser:newuser ~/.ssh /home/newuser
```

### TEST THAT NEWUSER WORKS
```console
su - newuser
```
```console
whoami
```
Output should say: ```newuser```   

***Test that newuser has sudo permissions***
```console
sudo ls -lah /root
```
You'll get an output asking for a password:
```console
[sudo] password for newuser:
```
Enter the password for newuser and you will see the /root directory listing.   
Exit the newuser shell by entering ```exit``` or ```ctrl+d``` 
 
### BACK TO ROOT SHELL, ENABLE AND CHECK FIREWALL STATUS
```ufw allow OpenSSH```
```ufw enable```
```ufw status```

### LOGOUT, RECONNECT AS NEWUSER
Let's reconnect to the droplet as ```newuser```
```ctrl+d``` OR ```exit```

### SSH AS NEW USER
```ssh -i ~/.ssh/id_oceanubu newuser@ip.address.x.x.x```

### SET UP GIT
```console
git config --global user.name 'First Last'
```
```console
git config --global user.email 'first_last@brown.edu'
```

### GIT-CREDENTIAL-MANAGER (v2.4.1 as of 2024-01-09)
***(Git Credential Manager is a credential helper for secure authentication with GitHub and multifactor authentication)***
```console
wget https://github.com/git-ecosystem/git-credential-manager/releases/download/v2.4.1/gcm-linux_amd64.2.4.1.deb
```
```console
sudo dpkg -i gcm-linux_amd64.2.4.1.deb
```
```console
git config --global credential.credentialStore gpg
```

```console
git-credential-manager configure
```
```console
git config --global credential.credentialStore gpg
```
```console
gpg --default-new-key-algo rsa4096 --gen-key
```

***(ENTER NAME, EMAIL, OK; ADD PASSWORD TO KEY)<br>***
*save your gpg password somewhere safe as you will be asked for it at least once when interacting with github)*
We need to note, and copy some of this information, enter:
```console
gpg --list-secret-keys --keyid-format=long
```
```console
home/newuser/.gnupg/pubring.kbx
------------------------------
sec   rsa4096/JJMVNZ8ERAZZILBW 2024-01-11 [SC] [expires: 2026-01-10]
      H2ZJYTQLNDOX7ZDYRBEMUOGIY06E4CE2C5GCS8RS
uid                 [ultimate] NEW USER <new_user@brown.edu>
ssb   rsa4096/KUHXODP27VUUHIWN 2024-01-11 [E] [expires: 2026-01-10]
```
We care about the 16 character string after
 ```sec   rsa4096/```
that is:
 ```JJMVNZ8ERAZZILBW``` 
Copy that string. 
### PASS (THE STANDARD UNIX PASSWORD MANAGER)
```console
sudo apt install pass -y```<br>

```console
pass init JJMVNZ8ERAZZILBW
```

### CREATE SSH KEY FOR GITHUB 
```console
ssh-keygen -t ed25519 -C "new_user@brown.edu"
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/srdkr/.ssh/id_ed25519): id_ghsshkey
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_ghsshkey
Your public key has been saved in id_ghsshkey.pub
The key fingerprint is:
SHA256:H37FHP0Z5nRuddPvdMzQW7n59vi+SRjhml/uVrmo0mY cody_carvel@brown.edu
The key's randomart image is:
+--[ED25519 256]--+
|                 |
|               oo|
|             .o*O|
|            .o=*#|
|        S .  o+*@|
|         o .o.o=+|
|          +o...o*|
|         . E..+=o|
|          +...+B=|
+----[SHA256]-----+
```
***(Tell the system to use this new ssh key)***
```console
eval "$(ssh-agent -s)"
```
```console
ssh-add ~/.ssh/id_ed25519
```
***(And ensure it is used everytime the user logs in by adding it to the .bashrc file)***
```
nano ~/.bashrc
```
*Add the following to the end of your .bashrc file*

```console
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

***(Copy the public key via cat command)***
```console
cat ~/.ssh/id_ghsshkey.pub
```
*copy output*
```
ssh-ed25519 dIH2Xzy9lA4C1wNvBSG3CmEtiKOTZDGIS77vLNYMsLoUGTL0lL33tO6CJmBDGIPPdIlll new_user@brown.edu
```

***(Add public key to github)***
[Add a new key](https://github.com/settings/ssh/new)
***(Give the key a title, type=authentication key, paste your copied public key in the key box)***

### GIT CLONE SR\_DKR\_STARTUP REPO; BUILDING IMAGES; STARTING CONTAINERS

```console
git clone https://github.com/Brown-University-Library/sr_dkr_startup.git```
```
```console
Cloning into 'sr_dkr_startup'...
Select an authentication method for 'https://github.com/':
  1. Device code (default)
  2. Personal access token
option (enter for default): 1
To complete authentication please visit https://github.com/login/device and enter the following code:
XXXX-XXXX
```
```
cd sr_dkr_startup
```

