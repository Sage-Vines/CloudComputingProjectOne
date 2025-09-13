**1. Overview** <br>
This project implements a simple REST web service that converts weight from pounds (lbs) to kilograms (kg). The web service is deployed on an Amazon EC2 AMI instance and is accessable via an HTTP GET request. This get request uses the /convert endpoint. For example, "/convert?lbs=150" would be part of a valid HTTP GET request. The goal of this project is to demonstrate the proper provisioning of a secure cloud VM in Amazon AWS, deploy a reliable REST API, and apply basic DevOps practices for the weight converter web service. 

**2. Architecture** <br>
- AWS EC2 AMI instance: Amazon Linux 2023 kernel-6.12 AMI
- Instance: t3.micro (free tier eligible)
- Node.js w/ express framework - Handles HTTP requests & Routing
- Morgan Middleware - Allows request logging
- systemd Service - Ensures REST web application will survive reboot and restart with crashes

**Data Flow** <br>
1. A client will send an HTTP GET request to http://localhost:8080/convert?lbs=<value> or http://3.17.207.19:8080/convert?lbs=<value>
2. The Express application will validate the query parameter and then convert the value from pounds into kilograms. The Express application will use the formula kg = lbs * 0.45359237 and return a JSON response. 
3. Error codes will be returned with the appropriate HTTP status codes

**3. Design Decisions** <br>
- Node.js w/ Express was chosen for its quick and easy setup. It also allowed for REST endpoints to be built with minimal overhead.
- Systemd was chosen to keep the sustem running even after it reboots and to automatically start in failure.
- Morgan middleware was added to make the log requests more structured.


- Error Handling:
    - 400 for a bad request is returned if the lbs parameter is missing or the lbs parameter is not a number.
    - 422 for an unprocessable entity is returned if the lbs parameter is negative or not finite.




**4. Testing Approach** <br>


**5. Cost & Cleanup** <br>

