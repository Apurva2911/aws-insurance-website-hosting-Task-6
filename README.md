🌩️ Host a Web App on AWS EC2 using VPC and Apache

This project shows how to create a secure network (VPC) and deploy a static website hosted on an EC2 instance with Apache web server.

🚀 Step 1: Create a Virtual Private Cloud (VPC)

Go to the AWS Management Console → VPC Dashboard.

Click Create VPC.

Select VPC and more.

Set:

Name: Insurance-VPC

IPv4 CIDR block: 10.0.0.0/16

Tenancy: Default

Click Create VPC.

✅ You now have a private cloud network.

🌐 Step 2: Create and Attach an Internet Gateway (IGW)

From the VPC Dashboard, select Internet Gateways → Create Internet Gateway.

Name it Insurance-IGW.

Click Create Internet Gateway.

Select your new IGW → Actions → Attach to VPC → Choose Insurance-VPC.

✅ This allows your instances in the VPC to connect to the internet.

🧩 Step 3: Create a Public Subnet

Go to Subnets → Create Subnet.

Choose:

VPC: Insurance-VPC

Subnet name: Public-Subnet

Availability Zone: (e.g., us-east-1a)

IPv4 CIDR block: 10.0.1.0/24

Click Create Subnet.

After creation → select subnet → Edit subnet settings → Enable Auto-assign public IPv4 → Save.

✅ This subnet can host your web server accessible from the internet.

🗺️ Step 4: Configure Route Table

Go to Route Tables → Create route table.

Name: Public-Route-Table

Select VPC: Insurance-VPC

Click Create Route Table.

Select your new table → Go to Routes tab → Edit routes.

Add:

Destination: 0.0.0.0/0

Target: Select your Insurance-IGW

Save routes.

Go to Subnet associations → Edit subnet associations → Add Public-Subnet → Save.

✅ This ensures traffic from your subnet can reach the internet.

💻 Step 5: Launch an EC2 Instance

Go to EC2 Dashboard → Launch Instance.

Name: Insurance-Web-Server

Choose Amazon Linux 2 AMI.

Instance type: t2.micro (Free tier).

Create or choose a key pair:

Example name: 27october.pem

Network settings:

Select VPC: Insurance-VPC

Select Subnet: Public-Subnet

Enable Auto-assign Public IP

Configure Security Group:

Create new SG Web-SG

Add inbound rules:

HTTP (port 80) → Source: Anywhere (0.0.0.0/0)

SSH (port 22) → Source: My IP

Click Launch Instance.

✅ This starts a virtual machine inside your network.

🔌 Step 6: Connect to EC2 using SSH

Go to EC2 → Instances → Select instance.

Copy the Public IPv4 DNS or IP Address.

Open a terminal where your .pem file is downloaded.

Run these commands:

cd Downloads
chmod 400 27october.pem
ssh -i "27october.pem" ec2-user@<public-ip-address>


Example:

ssh -i "27october.pem" ec2-user@3.235.126.146.compute-1.amazonaws.com


✅ You are now inside your EC2 instance.

⚙️ Step 7: Install and Start Apache Web Server
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd


Check the status:

sudo systemctl status httpd


✅ Apache web server is running!

🧭 Step 8: Move to Website Directory
cd /var/www/html


Use the ls command to view files.

📝 Step 9: Create HTML and CSS Files
Create HTML file
sudo nano index.html


Paste this code:

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SecureLife Insurance</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <div class="logo">🛡️ SecureLife</div>
        <nav>
            <a href="#">Home</a>
            <a href="#">Plans</a>
            <a href="#">About</a>
            <a href="#">Contact</a>
        </nav>
    </header>

    <section class="hero">
        <h1>Protect What Matters Most</h1>
        <p>Your life, health, and peace of mind — secured with us.</p>
        <a href="#" class="btn">Explore Plans</a>
    </section>

    <section class="plans">
        <h2>Our Insurance Plans</h2>
        <div class="plan-card">
            <h3>🏠 Home Insurance</h3>
            <p>Comprehensive protection for your home and property against unexpected damage.</p>
        </div>

        <div class="plan-card">
            <h3>🚗 Vehicle Insurance</h3>
            <p>Stay covered on every journey with complete car and bike insurance solutions.</p>
        </div>

        <div class="plan-card">
            <h3>❤️ Life Insurance</h3>
            <p>Secure your family’s future with flexible and affordable life insurance policies.</p>
        </div>

        <div class="plan-card">
            <h3>💼 Business Insurance</h3>
            <p>Protect your business assets, employees, and operations from unforeseen risks.</p>
        </div>
    </section>

    <footer>
        <p>© 2025 SecureLife Insurance. All rights reserved.</p>
    </footer>
</body>
</html>

Press Ctrl + O, Enter, Ctrl + X to save.

Create CSS file
sudo nano style.css

Paste this:

body {
  margin: 0;
  font-family: "Poppins", sans-serif;
  background: linear-gradient(180deg, #f0f9ff, #e0f7fa);
  color: #333;
}

/* Header */
header {
  background: linear-gradient(90deg, #ff6a00, #ee0979);
  color: white;
  padding: 18px 60px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}

header .logo {
  font-size: 1.8rem;
  font-weight: 700;
}

header nav a {
  color: white;
  margin-left: 25px;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s;
}

header nav a:hover {
  color: #ffd166;
}

/* Hero Section */
.hero {
  text-align: center;
  background: linear-gradient(135deg, #7b4397, #dc2430);
  color: white;
  padding: 120px 20px;
}

.hero h1 {
  font-size: 3rem;
  margin-bottom: 15px;
}

.hero p {
  font-size: 1.2rem;
  margin-bottom: 25px;
}

.btn {
  background: linear-gradient(90deg, #06beb6, #48b1bf);
  color: white;
  padding: 12px 30px;
  border-radius: 30px;
  text-decoration: none;
  font-weight: bold;
  transition: 0.3s;
}

.btn:hover {
  background: linear-gradient(90deg, #43cea2, #185a9d);
}

/* Services Section */
.services {
  padding: 70px 30px;
  text-align: center;
  background-color: #ffffff;
}

.services h2 {
  font-size: 2.2rem;
  margin-bottom: 30px;
  color: #ff6a00;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 25px;
}

.card {
  background: linear-gradient(135deg, #ffecd2, #fcb69f);
  width: 280px;
  padding: 25px;
  border-radius: 20px;
  box-shadow: 0 5px 10px rgba(0,0,0,0.15);
  transition: transform 0.3s, box-shadow 0.3s;
}

.card:hover {
  transform: translateY(-10px);
  box-shadow: 0 10px 20px rgba(0,0,0,0.25);
}

.card h3 {
  color: #d7263d;
  font-size: 1.3rem;
}

/* About Section */
.about {
  padding: 70px 30px;
  background: linear-gradient(90deg, #74ebd5, #acb6e5);
  text-align: center;
  color: #333;
}

.about h2 {
  color: #000;
}

/* Contact Section */
.contact {
  padding: 60px 20px;
  background: linear-gradient(120deg, #89f7fe, #66a6ff);
  color: white;
  text-align: center;
}

/* Footer */
footer {
  background: linear-gradient(90deg, #ff6a00, #ee0979);
  color: white;
  text-align: center;
  padding: 15px;
  font-size: 0.9rem;
}

/* Responsive Design */
@media (max-width: 768px) {
  header {
    flex-direction: column;
    text-align: center;
  }

  header nav a {
    display: inline-block;
    margin: 10px;
  }

  .hero h1 {
    font-size: 2.2rem;
  }

  .card-container {
    flex-direction: column;
    align-items: center;
  }

  .card {
    width: 90%;
  }
}

Save and exit.

🌍 Step 10: Test Your Website

Open your browser and visit:

http://<your-public-ip>

Example:

http://44.203.230.27

✅ You should now see your colorful insurance website hosted on AWS!

🧾 Step 11: Verify Apache Auto Start

To ensure Apache runs on every reboot:

sudo systemctl enable httpd

🧠 Optional Commands Explained
Command	Purpose
chmod 400	Sets secure permissions for your .pem file
ssh -i	Connects securely to EC2 using private key
sudo yum install httpd -y	Installs Apache web server
sudo systemctl start httpd	Starts the Apache service
sudo nano filename	Opens file editor in terminal
ls	Lists files in current directory
✅ Project Outcome

You have successfully:

Created a secure AWS VPC environment

Configured subnets, IGW, and route tables

Launched and connected to an EC2 instance

Installed and configured Apache

Deployed a colorful insurance website
