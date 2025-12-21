Week 1 Homework — Custom Webserver on AWS EC2
---
This guide walks you through creating a custom webserver hosted on an AWS EC2 instance.
Follow each step carefully and you’ll have your own running webserver serving your custom HTML.

---

### Task 

HW - Class 7 Homework
Screenshot of running WebServer
Copy of the start up script saved as a .sh file
README.md instructions on how to configure the EC2 with teardown instructions.

---
1. ### Create a Security Group

- Log in to AWS Console

- Access the EC2 dashboard. (There are a few ways to do this)

- On the left side of the screen, scroll down to Network and Security sub-heading and click on Security Groups

- On Security groups page, click Create Security group

- Enter Basic details as follows:

    -Security group name: class-7-loves-lizzo
    -Description: class-7-loves-lizzo
    -VPC: <default> Leave as default vpc.

- Scroll down to Inbound rules and click on Add rule.

- Add rules as follows:

    - Type: HTTP
    - Protocol: TCP
    - Port range: 80
    - Source: Anywhere-IPv4
    - Description: Optional <HTTP>

-DO NOT MAKE ANY CHANGES TO OUTBOUND RULES OR YOU WILL BE TEABAGGED BY CHEWBACCA!!!

-Tags (optional)

    -Key: name
    -Value: class-7-wk1-homework

- Click Create security group

- Confirm security group details

---

2. ### Launch an EC2 Instance

- On the EC2 Dashboard, click Launch instance
- In Name and tags, enter the name of your instance.
- in Application and OS Images, leave as default. Amazon Linux recommended
- In Instance type, leave default t3.micro
- In Key pair(login), click on "*Create new key pair*"
- Create key pair as follows:
    - Key pair name: class-7-wk1-homework
    - Key pair type: RSA
    - Private key file format: .pem
- click Create key pair and save key on your local drive for later access
-  in Network settings, highlight Select existing security group and select the security group created in step 1 above.
- Leave configure storage settings as default.
- Expand the Advanced details window and scroll all the way down to user data. 



3. ### Add User Data

-Paste your custom script in the User data (optional)window and click on Launch instance.This is the script that loads whenever the instance starts.

4. ### Verify your instance

- Once the Launch instance process completes, return to your Instances page to confirm view your Instance state shows up as Running.
- Click on your instance link in the Instance ID column.
- Your running instance should show up as below
- 
5. ### Access your Webserver

- Click on the double-square icon labeled Public DNS to copy the DNS address of your instance. you shold get a notice indicating that the details were copied to your clipboard.
- Open and new browser sindow and in the address bar type "http://" and paste the contents of you clipboard. 
Hit Enter on your keyboard and your browser should open the website you just created.