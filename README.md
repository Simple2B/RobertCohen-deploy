# AWS EC2 — create EC2 instance

## AWS Account
You can sign up for a free tier account if you don’t already have an account with AWS. AWS provides a free tier for 12 months for certain types of services and EC2 instances.

## Creating EC2 Instance
1. Login to the AWS console. Initially Recently visited section will be blank.

<img width="1009" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/f81c4087-67b1-4fa0-aaf6-081e795f3138">

2. Go to Search bar and type EC2, Select the EC2 from available options as selected here -

<img width="875" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/eb723cb4-bdd0-455f-8666-f35da3c76b43">

3. Click on the `Launch Instance` button (Highlighted in Orange)

<img width="856" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/0bbe832b-f6f3-4c65-a5a3-36c56983738d">

4. Provide a name for instance — “userbook-app-instance”

<img width="858" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/3a91927e-f6d6-4685-91ed-2bf4a6dae1e9">

5. Under the option Application and OS Images (Amazon Machine Image), select Amazon Linux as selected in the above image.
   
6.  Create a new key pair by clicking on the highlighted button “Create new key pair”

<img width="782" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/ef3d0a57-4e61-47a2-8083-6a12be6b637c">

Provide Key pair name — “ub-key-pair”

<img width="685" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/7da0f20c-66b0-4fbf-b7ab-44ff535dd3f2">

Click on Create key pair. A file with the name `ub-key-pair.pem` will be downloaded to your system. 
!This file will be used later to connect this EC2 instance via SSH

7. Change Network Settings — change settings as per the below image.

<img width="635" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/0f6750a4-ecd1-4e6e-bdb1-84901dceb76b">

8. Now click on Launch Instance(In Orange) at right bottom corner

<img width="659" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/e7c2f8d5-78cd-40fb-84f2-d50fdcc995af">

Instances should be ready after a few seconds.

<img width="626" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/7a29e0a7-49d2-4f9f-b80e-088bbf9b453e">

9. Click on the instance ID from the success message.
Alternatively, we can use EC2 -> Instances

<img width="754" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/2ab54400-2757-4311-9b26-cffc418e817b">

Now your instance is ready. It is time to connect to this instance. Connection to EC2 instance via SSH

1. Open the Instance summary by clicking on Instance ID

<img width="868" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/828f60a7-4de2-48cb-aaa5-895df9771e1f">

2. Click Connect (on the right) as shown above.
   
3. Connect to instance window will open with 4 tabs. Click the SSH client tab.

<img width="617" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/cee00983-53a5-4845-a5a6-e69ec79f8ecd">

4. Perform the steps as mentioned in your SSH client tab.

   Step 1 — The SSH client in your machine can be a `terminal`, `cmd` (Windows command line), or any other tool like `putty`.
   
   Step 2 — Go to the folder from the terminal where the .pem file was downloaded.
   
   Step 3 — Run the command

   ```bash
   chmod 400 ub-key-pair.pem
   ```
   
   Step 4 — Run command ssh -i “ub-key-pair.pem” <address from you ssh client window>
   

<h2>Installing NGINX on Amazon Linux EC2</h2>

1. SSH to Amazon Linux EC2 Instance

   ```bash
   ssh robert_cohen ##(host in config file)
   ```

2. Update Packages on Amazon Linux

   ```bash
   sudo yum update
   ```

3. Install NGINX on Amazon Linux

   ```bash
   sudo yum install nginx
   ```

4. Test Installation Using Bash
   Installation is done and let's do a small test to confirm that it is working.

   ```bash
   nginx -v
   ```
   
   ```bash
   sudo service nginx status
   ``` 

<h2>Configure Docker & Docker-Compose in AWS EC2</h2>

   Docker:
   
   ```bash
   sudo yum install docker -y
   ```
   
   ```bash
   sudo service docker start
   ```
   
   ```bash
   sudo chkconfig docker on ## make docker  autostart
   ```

   ```bash
   sudo reboot ## only if for you it is necessary
   ```

   Docker-Compose:
   
   ```bash
   sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose   ## docker-compose (latest version)
   ```
   
   ```bash
   sudo chmod +x /usr/local/bin/docker-compose  ## Fix permissions after download
   ```
   
   ```bash
   docker-compose version  ## Verify success
   ```

   <h2>Then you need to go to the settings and make your configuration (Amazon Linux EC2)</h2>

   ```bash
   cd /etc/nginx/conf.d/
   ```

   and create a configuration file 
   (in our case, these are three configuration files, that is, a file for the site itself - diemsondemand.conf, 
   a file for the API - api.diemsondemand.conf, 
   and a file for the admin panel - portal.diemsondemand.conf)

Contents of files: 

 1. diemsondemand.conf

    ```bash
       server {
          server_name diemsondemand.com;  - * add your domain name
              # Load configuration files for the default server block.
              include /etc/nginx/default.d/*.conf;
              location / {
                  proxy_pass http://localhost:3000;
                  proxy_set_header X-Real-IP  $remote_addr;
                  proxy_set_header X-Forwarder-For $proxy_add_x_forwarded_for;
                  proxy_set_header Host $host;
                  proxy_set_header X-Original-URL $request_uri;
                  proxy_set_header X-Forwarded-Proto $scheme;
                  proxy_http_version 1.1;
                  proxy_set_header Upgrade $http_upgrade;
                  proxy_buffering off;
              }
          listen 80;
       }
    ```

2. api.diemsondemand.conf

  ```bash
      server {
          server_name api.diemsondemand.com;  - * add your domain name
              # Load configuration files for the default server block.
              include /etc/nginx/default.d/*.conf;
              location / {
                  proxy_pass http://localhost:8002;
                  proxy_set_header X-Real-IP  $remote_addr;
                  proxy_set_header X-Forwarder-For $proxy_add_x_forwarded_for;
                  proxy_set_header Host $host;
                  proxy_set_header X-Original-URL $request_uri;
                  proxy_set_header X-Forwarded-Proto $scheme;
                  proxy_http_version 1.1;
                  proxy_set_header Upgrade $http_upgrade;
                  proxy_buffering off;
              }
              listen 80;
      }
   ```

3. portal.diemsondemand.conf

   ```bash
      server {
          server_name portal.diemsondemand.com; - * add your domain name
              # Load configuration files for the default server block.
              include /etc/nginx/default.d/*.conf;
              location / {
                  proxy_pass http://localhost:8001;
                  proxy_set_header X-Real-IP  $remote_addr;
                  proxy_set_header X-Forwarder-For $proxy_add_x_forwarded_for;
                  proxy_set_header Host $host;
                  proxy_set_header X-Original-URL $request_uri;
                  proxy_set_header X-Forwarded-Proto $scheme;
                  proxy_http_version 1.1;
                  proxy_set_header Upgrade $http_upgrade;
                  proxy_buffering off;
              }
          listen 80;
      }
   ```

Configure certbot for certificates

1. Update Package Manager:
   ```bash
   sudo yum update
   ```

2. Install Certbot:
   ```bash
   sudo yum install certbot
   ```

3. Install Certbot Nginx Plugin:
   ```bash
   sudo yum install python3-certbot-nginx
   ```

4. Obtain SSL Certificate:
   ```bash
   sudo certbot --nginx
   ```

! This command will guide you through the process of obtaining and installing SSL certificates for your Nginx setup. It will ask for your email address and domain name, and will handle the configuration of Nginx for SSL/TLS.

<h2>The next step is to run the project on Amazon Linux EC 2</h2>

```bash
## Create compose.yaml file
wget https://raw.githubusercontent.com/Simple2B/RobertCohen-deploy/main/compose.yaml 
```

```bash
##  Create .env file  and add to .env file variables
cat > .env
read -p "Press Enter to pass variables"
```

```bash
# Email configuration
MAIL_USERNAME=<sicret>
MAIL_DEFAULT_SENDER=diemsondemand.info@gmail.com
MAIL_PASSWORD=<sicret>
MAIL_SERVER=email-smtp.eu-north-1.amazonaws.com
MAIL_PORT=465
MAIL_USE_TLS=false
MAIL_USE_SSL=true

# Super admin
ADMIN_USERNAME=simple2b
ADMIN_EMAIL=simple2b.info@gmail.com
ADMIN_PASSWORD=<sicret>

# Flask app
NEXT_PUBLIC_LOGIN_URL=http://portal.diemsondemand.com/login

# Fast API
NEXT_PUBLIC_API_URL=http://api.diemsondemand.com/

read -p "Press enter then Control + D to continue"
```

```bash
docker-compose pull
```

```bash
docker-compose up -d
```

