# CloudComputingProjectOne
This project aims to deploy a small REST web service on Amazon AWS which will convert pounds/lbs to kilograms/kg.

- Amazon Machine Image (AMI) Choice: Amazon Linux 2023 kernel-6.12 AMI (Amazon Linux 2 is not under free tier)
- Instance Type: t3.micro (free tier eligible)
- Key Pair: ScarlettKey.pem
- VPC: vpc-0d1f953bf87d6fdce (default)
- Public URL/IP: http://3.17.207.19
- Security Group Summary: Custom Security Group - SageSecurityOne w/ Two Rules
  - Rule 1:
    - Type: ssh
    - Protocol: TCP
    - Port Range: 22
    - Source Type: My IP
    - Description: SSH
  - Rule 2:
    - Type: HTTP
    - Protocol: TCP
    - Port Range: 80
    - Source Type: Anywhere
    - Description: testing

**How to Install, Run, & Test**
- Log in to the Amazon AWS console and search for EC2 Virtual Servers in the Cloud
- Press the button to launch an instance
- Choose the Amazon Linux 2023 kernel-6.12 AMI
- Choose the t3.micro instance type
- Choose or create a RSA .pem file key pair - mine is named ScarlettKey.pem
- Create a security Group with the Security Group Summary rules located above
- Launch the AMI instance and wait for its instance state to be "Running"
- Ensure the user is ec2-user and not a root user
- Open Git Bash, Windows Subsystem for Linux, or another terminal (I used Git Bash)
- Navigate to the folder where your key pair file is located
- Use the chmod command "chmod 400 MyKeyPair.pem" (Mine was chmod 400 ScarlettKey.pem)
- Use the ssh command to ssh into the AMI instance "ssh -i MyKeyPair.pem ec2-user@<PUBLIC_IP>" (My case was ssh -i ScarlettKey.pem ec2-user@3.17.207.19)
- Use command "sudo yum update -y && sudo yum install -y nodejs npm" to update and install Node.js
- Use command "mkdir -p ~/p1 && cd ~/p1" to make a folder named p1 in the home directory
- Use command "npm init -y" to create a new Node.js project
- Use command "npm install express morgan" to add Express and Morgan for building a web server and handling HTTP requests

**EITHER**
- Type:
cat > server.js << 'EOF'
*Input text from server.js file*
EOF

**OR** 
- Using GitHub, obtain the HTTPS clone link
- Type: git clone https://github.com/Sage-Vines/CloudComputingProjectOne.git

**THEN**
- Use command "node server.js" to start listening on port 8080
- Open an additional terminal and ssh into the EC2 AMI instance the same way as before
- Use command "curl http://localhost:8080/convert?lbs=150" since you are ssh'd into the EC2 AMI instance

**Implementing a Service**

- Use command CTRL + C in the original terminal to cancel the port listening on port 8080 before implementing a service
- Use command "sudo nano /etc/systemd/system/p1.service"
- Input:

[Unit] <br>
Description=CS454 Project 1 REST Service <br>
After=network.target<br>
<br>
[Service] <br>
User=ec2-user <br>
workingDirectory=/home/ec2-user/p1 <br>
ExecStart=/usr/bin/node /home/ec2-user/p1/server.js <br>
Restart=always <br>
Environment=PORT=8080 <br>
<br>
[Install] <br>
WantedBy=multi-user.target <br>

- Save and exit from the edtior when you are finished
- Use command "sudo systemctl daemon-reload" to reload the configuration files
- Use command "sudo systemctl enable --now p1" to enable and start the custom service at the same time
- Use command "sudo systemctl status p1 --no-pager" to show the current status of the service named p1
- Use test cases to test small REST web service for converting pounds to kilograms


**Test Cases for Use**
- Happy path: http://localhost:8080/convert?lbs=0 --> 0.000 kg
- Typical: http://localhost:8080/convert?lbs=150 --> 68.039 kg
- Edge: http://localhost:8080/convert?lbs=0.1 --> 0.045 kg
- Error: http://localhost:8080/convert(missing parameter) --> 400 (Bad request if query parameter is missing or is not a number)
- Error: http://localhost:8080/convert?lbs=-5 --> 422 (Unprocessable Entity if value is negative or non-finite)
- Error: http://localhost:8080/convert?lbs=Nan --> 400 (Bad request if query parameter is missing or is not a number)

**Cleanup & Cost**
- Use command "sudo systemctl disable --now p1" in order to remove the p1 service
- Close the terminal(s) used to ssh
- Delete any unused EBS volumes to avoid storage charges
- Either stop the AMI instance (You will still be billed for associated resources like EBS volumes)
- Or terminate (delete) the instance and the root EBS volume will be deleted as well (I did this at the end of my project)

**Additional Notes**
- .gitignore was added so as to not pull/push/commit "node_modules"

