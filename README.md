# Setting-up Node JS Application on AWS Server

- Prerequisites:
    1. AWS EC-2 Instance with Amazon Linux
    2. Nginx Installed and running

- Steps:
    ## Setup Process Manager and Host Node Application
    1. Install Node using Node Version manager (NVM)
        - Pull the NVM Bash file
          `curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.0/install.sh | bash`
         - Execute the NVM Bash Script
             `. ~/.nvm/nvm.sh`
         - Install Node and NPM using NVM
                ```nvm install <NODE_VERSION_CODE>```
        - Verifying Node Installation
           ```node -e "console.log('Running Node.js ' + process.version)"```
    2. Install the Node Process Manager (PM2) using NPM:
            ```npm install -g pm2```
    3. Setup application startup script in Process Manager
            ```pm2 start ./server.js```
    4. Configure PM2 Startup Script
            ```pm2 startup systemd```
    5. Run the Command which was part of o/p of previous command
        `sudo env PATH=$PATH:/home/<USER_NAME>/.nvm/versions/node/v11.10.1/bin /home/<USER_NAME>/.nvm/versions/node/v11.10.1/lib/node_modules/pm2/bin/pm2 startup systemd -u <USER_NAME> --hp /home/<USER_NAME>`
    6. Start the Application on BootUp
            `systemctl status pm2-<USER_NAME>`
    7. List current Applications in Process Manager
            `pm2 list`
    10. Verify application with following Command (You should see your o/p on Terminal)
            `curl http://localhost:<PORT_NAME>`

    ## Setup Nginx with reverse Proxy
    1. Replace Following Block in Nginx Conf file
            
               location / {
                }
            
        with following Block
            
            location / {
                    proxy_pass http://localhost:<PORT_NAME>;
                    proxy_http_version 1.1;
                    proxy_set_header Upgrade $http_upgrade;
                    proxy_set_header Connection 'upgrade';
                    proxy_set_header Host $host;
                    proxy_cache_bypass $http_upgrade;
                }

    2. Restart Nginx with following command
        `sudo systemctl restart nginx`
    
    ## Handy PM Commands
    - ```pm2 list```: List all the PM2 Applications
    - ```pm2 start <APP_NAME>```: Start the PM2 Application
    - ```pm2 stop <APP_NAME>```: Stop the PM2 Application
    - ```pm2 restart <APP_NAME>```: Restart the PM2 Application
