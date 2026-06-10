# Cloud Computing & DevOps — Task 2
## Containerization Using Docker & Cloud Deployment

### 🚀 Live Application URL
* **Production Link:** [http://13.60.66.17](http://13.60.66.17)

---

### 📝 Project Overview
This project demonstrates a complete, industry-standard DevOps pipeline. I have successfully containerized a static portfolio website using a lightweight Nginx server wrapper and deployed it onto an active cloud-hosted virtual machine using AWS EC2.

---

### 🛠️ Tools & Technologies Used
* **Docker:** For creating application blueprints, compiling images, and running isolated containers.
* **Nginx:** Utilized as a lightweight, high-performance web server inside the container to serve static assets.
* **AWS EC2:** Serving as the Ubuntu Linux-based cloud virtual server infrastructure.
* **SCP (Secure Copy Protocol):** Used to securely transfer local project files directly to the cloud instance.
* **Linux Terminal & Nano:** Utilized for executing remote system configurations, dealing with environment permissions, and file creation.

---

### 📁 Project File Structure
```text
portfolio-website/         
├── index.html          
├── styles.css          
├── image_d5e76f.png
└── Dockerfile

🚀 Step-by-Step Deployment Documentation
1. Local Application Setup
The project structure containing the portfolio web assets (index.html, styles.css, and images) was verified locally on the workstation.

2. Cloud Server Provisioning (AWS EC2)
Launched an active Ubuntu Server instance on the AWS EC2 dashboard.
Assigned the t2.micro instance type to stay within the AWS Free Tier scope.
Generated and downloaded a secure .pem key pair for remote cryptographic login verification.
3. Data Transfer via SCP
Using Windows PowerShell from the local machine's Downloads directory, the entire project folder was securely pushed over the network to the cloud server's root directory:

Bash

scp -i your-key.pem -r portfolio-website ubuntu@13.60.66.17:/home/ubuntu/
4. Environment Setup on Cloud Server
Connected securely to the EC2 instance using SSH and set up the Docker run engine:

Bash

# Connect to the cloud instance
ssh -i your-key.pem ubuntu@13.60.66.17# Synchronize internal package indexes
sudo apt update# Install and activate the Docker core platform
sudo apt install docker.io -y
sudo systemctl start docker
5. Dockerfile Integration & Permission Resolution
Navigated into the uploaded directory to generate the Docker engine instructions blueprint. Encountered a standard Linux file permission restriction block, which was successfully bypassed by executing the text interface editor with elevated sudo rights:

Bash

cd portfolio-website
sudo nano Dockerfile
Dockerfile Blueprint Content Added:

Dockerfile

FROM nginx:alpineCOPY . /usr/share/nginx/htmlEXPOSE 80
Saved changes and safely closed the interface using Ctrl + O, Enter, and Ctrl + X.

6. Building & Orchestrating the Production Container
Compiled the image blueprint locally on the host virtual file system and launched the background service worker:

Bash

# Compile the custom portfolio image
sudo docker build -t portfolio-website .# Start the background container mapped to production web port 80
sudo docker run -d -p 80:80 portfolio-website
7. Firewall Network Configuration (AWS Security Group)
Encountered an initial ERR_CONNECTION_TIMED_OUT error when trying to reach the application. Diagnosed this as an inbound networking blockade on the cloud infrastructure layer.

Opened the AWS EC2 Console -> Security Groups.
Modified the Inbound Rules settings.
Added a new firewall permission allowing HTTP traffic on Port 80 from Anywhere-IPv4 (0.0.0.0/0).
Applied changes, which instantly brought the website live to the public internet.
📊 Verification Proof
Running Container Metadata (sudo docker ps):
Plaintext

CONTAINER ID   IMAGE               COMMAND                  CREATED         STATUS         PORTS                NAMES
137dfc24e794   portfolio-website   "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   

