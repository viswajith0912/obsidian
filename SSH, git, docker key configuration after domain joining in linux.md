sudo find /home -type d -name ".ssh"  

sudo cp -r /home/bridge/.ssh /home/user[@bridge-delivery.com/](http://greeshma.ps@bridge-delivery.com/)
sudo chown -R 548001476:548000513 /home/user[@bridge-delivery.com/.ssh](http://greeshma.ps@bridge-delivery.com/.ssh)
sudo chmod 700 /home/user[@bridge-delivery.com/.ssh](http://greeshma.ps@bridge-delivery.com/.ssh)
sudo chmod 600 /home/user[@bridge-delivery.com/.ssh/*](http://greeshma.ps@bridge-delivery.com/.ssh/*)

=============================================
  
Optional:   check this in Ad profile  
  
gpg --list-secret-keys --keyid-format LONG  

When we try to commit, it will give gpg issue  
  
Checking the key details in old profile-here bridge  
  
sudo -u bridge gpg --list-secret-keys --keyid-format LONG  
  
Here the key is 49D7B26785B73997 ,so use like this  
  
sudo -u bridge gpg --export-secret-keys 49D7B26785B73997 > /tmp/mykey.asc  
  
gpg --import /tmp/mykey.asc  

gpg --edit-key 49D7B26785B73997

> trust

> 5 (ultimate)

> quit  
  
Cross check if keys are there

gpg --list-secret-keys --keyid-format LONG  
  
Now try commit