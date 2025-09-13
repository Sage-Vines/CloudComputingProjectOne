# CloudComputingProjectOne
This project aims to deploy a small REST web service on Amazon AWS which will convert pounds/lbs to kilograms/kg.

-Amazon Machine Image (AMI) Choice: Amazon Linux 2023 kernel-6.12 AMI (Amazon Linux 2 is not under free tier)
-Instance Type: t3.micro (free tier eligible)
-Key Pair: ScarlettKey.pem
-VPC: vpc-0d1f953bf87d6fdce (default)
-Security Group: Custom Security Group - SageSecurityOne w/ Two Rules
  -Rule 1:
    -Type: ssh
    -Protocol: TCP
    -Port Range: 22
    -Source Type: My IP
    -Description: SSH
  -Rule 2:
    -Type: HTTP
    -Protocol: TCP
    -Port Range: 80
    -Source Type: Anywhere
    -Description: testing
