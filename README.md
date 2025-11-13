Deploying an Application on AWS EC2
Prerequisites
Before deploying your application on AWS EC2, ensure you have:

An AWS account
AWS CLI installed and configured (aws configure)
SSH key pair (.pem file) for EC2 access
A pre-built application ready for deployment

Step 1: Launch an EC2 Instance
Log in to AWS Management Console.
Navigate to EC2 > Instances.
Click Launch Instance.
Choose an Amazon Machine Image (AMI), e.g., Amazon Linux 2.
Select an instance type (e.g., t2.micro for free-tier eligibility).
Configure security group:
Allow SSH (port 22) for access.
Allow necessary ports (e.g., 80, 443 for web apps).
Add a key pair for SSH access.
Launch the instance.


Step 2: Connect to the EC2 Instance
ssh -i your-key.pem ec2-user@your-ec2-public-ip


Step 3: Update and Install Dependencies
sudo yum update -y   # Amazon Linux
sudo apt update -y   # Ubuntu/Debian

# Install required packages (example for a web app)
sudo yum install -y git nginx nodejs

Step 4: Deploy Your Application
Clone your application repository:
git clone https://github.com/your-repo/app.git
cd app
Install dependencies:
npm install  # For Node.js apps
Start the application:
node index.js &


Step 5: Configure Firewall and Security Group
Ensure your EC2 instance allows inbound traffic on required ports:

sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --reload
Modify security group in AWS to allow necessary ports.

Step 6: Set Up a Reverse Proxy (Optional)
For production deployments, use Nginx as a reverse proxy:

sudo nano /etc/nginx/nginx.conf
Configure the server block:

server {
    listen 80;
    server_name your-ec2-public-ip;
    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
Restart Nginx:

sudo systemctl restart nginx

<img width="1782" height="957" alt="Screenshot 2025-11-12 213131" src="https://github.com/user-attachments/assets/1d8d36bd-328e-4b91-b6b0-b9c33440c831" />



Step 7: Enable Auto-Start (Optional)
Use PM2 to keep your app running:

npm install -g pm2
pm2 start server.js
pm2 startup
pm2 save

Step 8: Access Your Application
Visit http://your-ec2-public-ip in a browser.
<img width="616" height="279" alt="Screenshot 2025-11-12 213603" src="https://github.com/user-attachments/assets/bd456a1c-eaf3-4351-8786-8eb7c3af09a8" />

<img width="640" height="383" alt="Screenshot 2025-11-12 213549" src="https://github.com/user-attachments/assets/c25cefc5-ff84-4490-8c2a-5e21c6fa765f" />

