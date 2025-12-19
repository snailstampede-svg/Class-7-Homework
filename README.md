

AWS

-----

# Security Groups:

1. Build a Security Group (Port 22 and 80)

-Name: wolfpack-wednesday

-Description: wolfpack-wednesday

-vpc: Leave as default

Inbound Rules
-Add Rule
-type SSH | Protocol: TCP | Port Range: 22 | Source Type: Anywhere IPv4 | Source: Default (0.0.0.0) | Description: SSH

-Add Rule
-type HTTP | Protocol: TCP | Port Range: 80 | Source Type: Anywhere IPv4 | Source: Default (0.0.0.0) | Description: HTTP

Outbound Rules
-DO NOT Touch OR YOU WILL BE STUCK IN A PIT OF HELL WITH KEISHA FOR ETERNITY!!!

Tags
-Key: Name
Value: wolfpack wednesday

+++++++++++++++++++++++++++++

2. Build an EC2 Instance (Amazon Linux based 2023)

    -Click Orange button "Launch Instance"
    Name: wolfpack wednesday

    Application and OS Images (Amazon Machine Image)
    -Click the "Quick Start" tab
    -Ensure "Amazon Linux" icon is selected
    -Amazon Machine Image: "Amazon Linux 2023 kernel-6.1 AMI" (default)
   - Description: LEave as default
   -Instance Type: t3.micro


3. Build a Key Pair

- Key Pair (login)
Name: wolfpack wednesday
Key pair type: RSA
-Private key file formate: .pem
Click Orange button (Create key pair)
-Save key pairs in Downloads folder on your local computer
-Note: Key pair name is now popultted with "wolfpack wednesday"

Network Settings:
Network: Default
Subnet: Default
Auto-assign public IP: should be defaulted as "Enable"

Firewall (Security Groups):
-Click "Select existing security group"
-In the section "Common security groups", scroll down and click previously created SG "wolfpack wednesday" 

Configure Storage: Default

Advanced Details:
-Scroll all the way down to section labeled "User data-optional" --skip everything before user data-optional

4. Copy and Paste Theo's EC2 Script

-Access the MookieWAF EC2Script via GitHub
-click the icon that looks like two pages together labeled "Copy Raw File" and ensure that the interface confirms that code was copied
- go back to user data in AWS and paste the script into the box.
-Pray to Chewbacca before you launch instance

5. Launch Instance

-Click Orange Button "Launch Instance"


6. Copy and Paste Public DNS

-In the green banner that indicates that instance was launched successfully, click on the instance id (looks like i-xxxxxxxx underlined). This will take you to the instance Summary page

- clikc the double boxes lableled "Public DNS" and ensure it alerts that Public DNS copied.
Open a new tab on your web browser and type: "http://, then paste your clipboard contents immediately after. Teh result should look similar to this: http://ec2-44-213-64-82.compute-1.amazonaws.com

-Press ENTER on your keyboard, the web page should load.




7. Paste "http:<public-dns>" - to show EC2 Script WebServer created.

 

8. SSH into EC2 Instance as it's running

- Go back to your instance summary page (the same page you copied and pasted your Public DNS)
-Click the blue "Connect" button at the top of the page.
- on the next page , leave everything as default and click the ocrange "connect" button athe athe bottom right of the page. This will open a command prompt for the instance. Look for the black bird at the top left with a command prompt below"Amazon Linux 2023"



9. Modify EC2 Script and refresh the WebServer page

On the command line type the following commands
```sh
sudo vim /var/www/html/index.html
```
- Press the letter "i" on your keyboard (lowercase ) this will put you into insert mode in VIM.


-Change the line "Samaurai KAtana" to "Wolfpack Wednesdays"

-Change the image source to :https://st3.depositphotos.com/2169563/14319/i/1600/depositphotos_143196005-stock-photo-young-beautiful-brazilian-woman-at.jpg

-press the "Esc" buttono on your keyboard

-type :wq and press Enter-->This saves the edits made to the file.


10. Ping IP address 8.8.8.8 (Google DNS) to show web connectivity.

-in the terminal window type 
```ping 8.8.8.8```
```CTRL+z OR CTRL+C```
 on your keyboard to end the ping streams

11. Complete Teardown

- on the instances page, highlight the instance you want to terminate, and select "terminate (delete) instance from the instance state dropdown and click  the Orange button that says Terminate (delete). This should terminate your instance after a short period.


- Go to your Security Group, click the Action button at the top and click "Delete Security Group"

- Praise Chewbacca and Get this Money







