Check if the Ubuntu machine is able to ping the DC server  :  
    nslookup [bridge-delivery.com](http://bridge-delivery.com)
    
- Set the hostname for the machine  
      
    Sudo hostnamectl set-hostname  [hostname.bridge-delivery.com](http://hostname.bridge-delivery.com)
    
- Verify if hostname is set properly  
    Hostnamectl
    
- Reboot the machine : sudo reboot
    
- Login back to bridge account
    
- Check for update: sudo apt update
    
- Install realm : sudo apt install realmd
    
- Join device to realm
    

Sudo realm join [bridge-delivery.com](http://bridge-delivery.com) -U nirmal.r

- Give admin rights to the user  
    sudo usermod -aG sudo [user@bridge-delivery.com](mailto:user@bridge-delivery.com)
    
- Enable offline logging
    

            sudo nano /etc/sssd/sssd.conf  
                                                            

             Add the line  : offline_credentials_expiration = 0

  

[domain/bridge-delivery.com]

default_shell = /bin/bash

krb5_store_password_if_offline = True

cache_credentials = True

offline_credentials_expiration = 0 <-here

krb5_realm = BRIDGE-DELIVERY.COM

realmd_tags = manages-system joined-with-adcli

id_provider = ad

fallback_homedir = /home/%u@%d

ad_domain = bridge-delivery.com

use_fully_qualified_names = True

ldap_id_mapping = True

access_provider = ad

  

Set permissions:

  

sudo chmod 600 /etc/sssd/sssd.conf

  

Restart SSSD:  
  
sudo systemctl restart sssd

  

- Creating home directory for the domain users while logging in  
      
    

 sudo apt update  
sudo apt install oddjob-mkhomedir -y

- Enable and start oddjobd service
    

sudo systemctl enable oddjobd --now

- Edit the PAM session configuration  
    sudo nano /etc/pam.d/common-session  
    Scroll to the bottom and add this line (if not already present):  
      
    

- session required pam_mkhomedir.so skel=/etc/skel/ umask=077
    

- Restart SSSD to apply changes: sudo systemctl restart sssd
    

  

sudo usermod -aG sudo user@bridge-delivery.com   

Let the Ad user to login 

  

- Copy file from old user to new AD user  
      
    sudo cp -a /home/olduser/* /home/[user@bridge-delivery.com](mailto:user@bridge-delivery.com)/
    

  

- Get UID & GID
    

Id [user@bridge-delivery.com](mailto:user@bridge-delivery.com)

Apply ownership

sudo chown -R 548001561:548000513 /home/nirmal.r@bridge-delivery.com/

sudo chmod -R u+rw /home/[nirmal.r@bridge-delivery.com](mailto:nirmal.r@bridge-delivery.com)/

  
  

Set directory execute permissions:

  

`sudo find /home/nirmal.r@bridge-delivery.com/ -type d -exec chmod u+x {} \;`

  

Add the domain user to the docker group:  
  
sudo usermod -aG docker user@bridge-delivery.com