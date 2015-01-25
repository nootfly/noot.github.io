---
layout: post
title:  "Build a git server on a Ubuntu server"
date:   2014-07-31 15:06:30+1000
categories: git
tags: git
---
1. Install open-ssh and create a git account

        sudo apt-get install openssh-server
        sudo useradd git
        sudo passwd git
        sudo mkdir /home/git/.ssh && sudo touch /home/git/.ssh/authorized_keys
       
       
2. Create the SSH Key Pair on a local server and upload the public key to the git server
        
        ssh-keygen -C "youremail@mailprovider.com"
        cat .ssh/id_rsa.pub | ssh git@servername "cat >> ~/.ssh/authorized_keys"
            
            
3. Update ssh configuration on the git server
      
      
         sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.factory-defaults
         sudo chmod a-w /etc/ssh/sshd_config.factory-defaults
         sudo gedit /etc/ssh/sshd_config
      
   change configuration to disable Password Authentication from:
   
         #PasswordAuthentication yes
   to:
      
        PasswordAuthentication no
4. Create a new git respository on the git server
  my-project.git is located in /home/git/
  
        git init --bare my-project.git
     
5. Add git remote and push souce codes to the remote server on a local machine

        git remote add origin git@servername:./my-project.git
        git remote set-url origin git@servername:./my-project.git
        git push origin master
        
        
Reference: https://www.digitalocean.com/community/tutorials/how-to-set-up-a-private-git-server-on-a-vps       
     