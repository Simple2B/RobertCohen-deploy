# AWS EC2 — create EC2 instance

## AWS Account
You can sign up for a free tier account if you don’t already have an account with AWS. AWS provides a free tier for 12 months for certain types of services and EC2 instances.

## Creating EC2 Instance
1. Login to AWS console. Initially Recently visited section will be blank.

<img width="1009" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/f81c4087-67b1-4fa0-aaf6-081e795f3138">

2. Go to Search bar and type EC2, Select the EC2 from available options as selected here -

<img width="875" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/eb723cb4-bdd0-455f-8666-f35da3c76b43">

3. Click on Launch Instance button (Highlighted in Orange)

<img width="856" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/0bbe832b-f6f3-4c65-a5a3-36c56983738d">

4. Provide name for instance — “userbook-app-instance”

<img width="858" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/3a91927e-f6d6-4685-91ed-2bf4a6dae1e9">

5. Under option Application and OS Images (Amazon Machine Image), select Amazon Linux as selected in above image.
   
6.  Create new key pair by click on highlighted button “Create new key pair”

<img width="782" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/ef3d0a57-4e61-47a2-8083-6a12be6b637c">

Provide Key pair name — “ub-key-pair”

<img width="685" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/7da0f20c-66b0-4fbf-b7ab-44ff535dd3f2">

Click on Create key pair. File with the name ub-key-pair.pem will be downloaded in your system. 
!This file will be used later to connect this EC2 instance via SSH

7. Change Network Settings — change setting as per below image

<img width="635" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/0f6750a4-ecd1-4e6e-bdb1-84901dceb76b">

8. Now click on Launch Instance(In Orange) at right bottom corner

<img width="659" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/e7c2f8d5-78cd-40fb-84f2-d50fdcc995af">

Instances should be ready after a few seconds.

<img width="626" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/7a29e0a7-49d2-4f9f-b80e-088bbf9b453e">

9. Click on the instance ID from the success message.
Alternatively we can use EC2 -> Instances

<img width="754" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/2ab54400-2757-4311-9b26-cffc418e817b">

Now your instance is ready. It is time to connect to this instance.
Connect to EC2 instance via SSH

1. Open Instance summary by click on Instance ID

<img width="868" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/828f60a7-4de2-48cb-aaa5-895df9771e1f">

2. Click Connect (in right) as shown above.
   
3. Connect to instance window will open with 4 tabs. Click the SSH client tab.

<img width="617" alt="image" src="https://github.com/Simple2B/RobertCohen-deploy/assets/63252239/cee00983-53a5-4845-a5a6-e69ec79f8ecd">

4. Perform the steps as mentioned in your SSH client tab.

   Step 1 — The SSH client in your machine can be terminal, command line or any other tool like putty.
   
   Step 2 — Go to the folder from the terminal where the .pem file was downloaded.
   
   Step 3 — Run command chmod 400 ub-key-pair.pem
   
   Step 4 — Run command ssh -i “ub-key-pair.pem” <address from you ssh client window>
   



## Create compose.yaml file

```bash
wget https://raw.githubusercontent.com/Simple2B/RobertCohen-deploy/main/compose.yaml
```
