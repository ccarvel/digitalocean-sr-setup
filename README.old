### INSTALL DOCKER-ONE-CLICK + UBUNTU 22.04 SERVER FROM DO MARKETPLACE

### ADD SSH-KEY VIA WEB PORTAL

### CONNECT VIA SSH AS ROOT 
```ssh -i ~/.ssh/id_key-using root@ip```

### APT UPDATE/UPGRADE
```sudo apt update && sudo apt upgrade -y```

### ADD A USER
```adduser cody```<br> 
***(ENTER PASSWORD, USER DETAILS, 2X)***

### ADD USER TO SUDO GROUP
```usermod -aG sudo cody```<br>
```usermod -aG docker cody```

### ENABLE, CHECK FIREWALL STATUS
```ufw enable```<br>
```ufw status```

### RSYNC ROOT .SSH DIR TO NEW USER
```rsync --archive --chown=cody:cody ~/.ssh /home/cody```

### LOGOUT, RECONNECT
```ctrl+d``` OR ```exit```

### SSH AS NEW USER
```ssh -i ~/.ssh/id_key-using cody@ip```

### SET UP GIT
```git config --global user.name 'Cody Carvel'```<br>
```git config --global user.email 'cody_carvel@brown.edu'```

### GIT-CREDENTIAL-MANAGER
```wget https://github.com/git-ecosystem/git-credential-manager/releases/download/v2.2.1/gcm-linux_amd64.2.2.1.deb```<br>
```sudo dpkg -i gcm-linux_amd64.2.2.1.deb```<br>
```git config --global credential.credentialStore gpg```<br>
```git-credential-manager configure```<br>
```gpg --gen-key```<br>
***(ENTER NAME, EMAIL, OK; ADD PASSWORD TO KEY)<br>***
```sudo apt install pass -y```<br>
```pass init Cody Carvel```

### GIT CLONE SR\_DKR\_STARTUP REPO
```git clone https://github.com/Brown-University-Library/sr_dkr_startup.git```<br>
```git branch -a``` <br>
***(HEALTH-FOR-SQL BRANCH)***<br>
```git checkout health-for-sql```

### REMOTE DESKTOP
```sudo apt install xfce4 xfce4-goodies -y```<br>
```sudo apt install xrdp -y```<br>
```sudo systemctl enable xdrp```<br>
```sudo systemctl start xrdp```<br>
```sudo ufw allow 3389/tcp```<br>
```sudo ufw reload```<br>



### QEMU 
```sudo apt install cpu-checker -y```<br>
```kvm-ok```<br>
```sudo apt update```
```sudo apt install qemu-kvm virt-manager virtinst libvirt-clients bridge-utils libvirt-daemon-system -y```<br>
```sudo systemctl enable --now libvirtd```<br>
```sudo systemctl start libvirtd```<br>
```sudo systemctl status libvirtd```<br>
```sudo usermod -aG kvm $USER```<br>
```sudo usermod -aG libvirt $USER```<br>

### TERMINAL-BASED SYSTEM INFO
```sudo apt install screenfetch -y```<br>
***From terminal*** ```screenfetch```
***displays sysinfo in ascii display, eg:*** 
<br>

```
                          ./+o+-       cody@cds-sr-ubudocker-vm
                  yyyyy- -yyyyyy+      OS: Ubuntu 22.04 jammy
               ://+//////-yyyyyyo      Kernel: x86_64 Linux 5.15.0-71-generic
           .++ .:/++++++/-.+sss/`      Uptime: 3h 36m
         .:++o:  /++++++++/:--:/-      Packages: 1230
        o:+o+:++.`..```.-/oo+++++/     Shell: bash 5.1.16
       .:+o:+o/.          `+sssoo+/    Resolution: 1920x965
  .++/+:+oo+o:`             /sssooo.   DE: Xfce
 /+++//+:`oo+o               /::--:.   WM: Xfwm4
 \+/+o+++`o++o               ++////.   WM Theme: Default
  .++.o+++oo+:`             /dddhhh.   GTK Theme: Greybird [GTK2]
       .+.o+oo:.          `oddhhhh+    Icon Theme: elementary-xfce-dark
        \+.++o+o``-````.:ohdhhhhh+     Font: Sans 10
         `:o+++ `ohhhhhhhhyo++os:      Disk: 16G / 78G (20%)
           .o:`.syhhhhhhh/.oo++o`      CPU: DO-Premium-Intel @ 2x 1.995GHz
               /osyyyyyyo++ooo+++/     GPU: Red Hat, Inc. Virtio GPU (rev 01)
                   ````` +oo+++o\:     RAM: 1100MiB / 3923MiB
                          `oo++.
                          ```
