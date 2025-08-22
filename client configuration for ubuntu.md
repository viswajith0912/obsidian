1. Connect to the portal by opening a web browser and navigating to:  
    [https://192.168.1.200:8090/  
      
    ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdzMU9--Tj-buYhGCTTBh4oFqRbZWRIgL8cZgCnhNrhGnAgoNp62AsOO8klJYVcdWrOSJVQwL9mWvLfSFsLc_ZMBcQaqDCzkzOtWTBvQznSKZtm03LaubVM5hH3AxoLlGmL0c1h?key=X6ct25roRRi_eP6K8_At9w)  
      
    ](https://192.168.1.200:8090/)
    
2. Click on Access the User Portal.  
      
    
3. In the Sophos User Portal screen, Log in using your Active Directory (AD) credentials.  
      
    
4. From the left side menu, select Download Client.  
      
    
5. Under the Authentication Clients section, click on Download for Linux x64  
    ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf7T1BVBCzToszXSM9slvifnr6h_oIOw2sCrXHE-1FM_gzDFdTeVNjwT_TwFtQGk7dW8dGKfL_kQyMgvXgXfVXLztCmBIkyQQamgzoXJDmHXFoV8rSIfjhyYpznCTSS5wKg55DE?key=X6ct25roRRi_eP6K8_At9w)  
      
    
6. Once the download is complete:
    
7. Open terminal
    
8. Hover over to Downloads ( cd Downloads)
    
9. Check and confirm that you have caa_x64.tar.gz  in downloads
    
10. Run the following command to extract the archive into your home directory  
    sudo tar -xzvf caa_x64.tar.gz -p -C $HOME
    
11.  You should see the following directories and files in your home directory:  
    /.caa/  
    /.caa/ca-cert.pem  
    /.caa/caa.conf  
    /.caa/README  
    /bin/  
    /bin/caa
    
12. Execute whoami in terminal  
    ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcz8uBGSObYBxAsXw9TDgWaKjjtDKZRuC1MJfJ5Q3nZk5m_pdGmnV5jX8us4iQ86ei0X4HEF3lSfdmObYIUhIxsMt5sbarnH5EVbP9h_OsGQrSdlDoncjaHYdCH7GHcEiR3mdFzhQ?key=X6ct25roRRi_eP6K8_At9w)
    
13. Note: Output of whoami will be your username
    
14. Execute the following commands: (Replace the username section with your username)  
      
    sudo mv ~/bin/caa /usr/local/bin  
    sudo chown -R $username /home/$username/bin  
    sudo chown -R $username /home/$username/.caa
    
15. Open /.caa/caa.conf in a text editor.  (Use any editors)  
    Eg: sudo gedit /home/$(whoami)/.caa/caa.conf
    
16. You should see the following details
    

             Copernicus host: 1.2.3.4  
            Username: USERNAME  
            Password: PASSWORD

17. Change this to.
    

             Copernicus Host: 192.168.1.200  
Username: Your Active Directory (AD) username  
Password: Your Active Directory (AD) password

18. Save and exit the text editor
    
19. Run caa in Terminal to start Client Authentication Agent.
    

  

Note: 

- Keep the terminal open to ensure the Captive Portal remains active.
    
- Whenever you need to use the Captive Portal, simply open the terminal and run the caa command to activate it.
    

  
  
  
  

To enable captive portal in startup service:

- Create a new service configuration file, using any text editor  
    sudo gedit /etc/systemd/system/captivelogin.service
    
- Enter the below configuration, Edit your username in the below  
    Note: Replace username with the output of whoami command
    

[Unit]

Description=Captive Client Authentication Agent (CAA)

After=network-online.target

Wants=network-online.target  

[Service]

Type=simple

ExecStart=/usr/local/bin/caa

ExecStartPre=/bin/sleep 10 

Restart=on-failure

RestartSec=10  

Environment=HOME=/home/username

Environment=CAA_CONFIG_PATH=/home/username/.caa/caa.conf 

StandardOutput=journal 

StandardError=journal 

SyslogIdentifier=caa   

[Install]

WantedBy=multi-user.target

  
  

- Save the File
    
- Execute the below commands to activate the new service:  
      
    sudo systemctl enable captivelogin.service  
    sudo systemctl start captivelogin.service
    
- If there is any error or the service is not started, close the terminal and reopen, then execute the following command:
    

             sudo systemctl restart captivelogin.service  
sudo systemctl status captivelogin.service
