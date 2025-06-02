# todoappfrontend
In this repo we are going to deploy todoapp frontend
this frontend code written in react

# 📝 Todo App - Full Deployment Guide

This document provides a complete setup guide for deploying the Todo App on Ubuntu using Node.js, React, and NGINX with private IP communication support for the backend.

---

## 📋 Prerequisites

Ensure your system or virtual machines have:

- **Ubuntu OS**
- **Node.js v16.x and npm**
- **NGINX Web Server**
- Network access between frontend and backend VMs

---

## 🚀 Step 1: Install Node.js and npm

Run the following commands to install Node.js (v16) and npm:

```bash
curl -s https://deb.nodesource.com/setup_16.x | sudo bash
sudo apt install nodejs -y
Verify installation:


node -v
npm -v
⚙️ Step 2: Update Backend API URL in Frontend
Open the file src/TodoApp.js in your frontend project and locate the line where the API base URL is defined.

Update it as follows:

javascript

const API_BASE_URL = 'http://<FrontendVM_PublicIP>/api';
Replace <FrontendVM_PublicIP> with your actual frontend server's public IP address.

📦 Step 3: Install Project Dependencies
From the root of your frontend project, run:


npm install
This will download and install all required Node.js packages.

🛠️ Step 4: Build the Frontend App
Compile the React frontend for production:

bash
Copy
Edit
npm run build
This will generate static files in the build/ directory.

🌍 Step 5: Deploy to NGINX
# Copy Build Files to NGINX Web Root

sudo cp -r build/* /var/www/html/
Ensure the NGINX server block (/etc/nginx/sites-available/default) is pointing to /var/www/html as the root.

# Restart NGINX

sudo service nginx restart
Your frontend app should now be accessible via the public IP of your frontend VM.

🌐 Step 6: (Optional) Configure Private IP Proxy for Backend
If your backend is hosted on a private IP, set up NGINX on the frontend VM to reverse proxy API requests.

6.1 Edit NGINX Configuration

sudo nano /etc/nginx/sites-available/default
6.2 Add the Following Block Before root /var/www/html;
nginx

location /api {
   proxy_pass http://<PrivateIP_of_BackendVM>:8000;
   proxy_set_header Host $host;
   proxy_set_header X-Real-IP $remote_addr;
   proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
   proxy_set_header X-Forwarded-Proto $scheme;
}
Replace <PrivateIP_of_BackendVM> with your actual backend VM’s private IP address.

6.3 Restart NGINX
bash
Copy
Edit
sudo service nginx restart
✅ Final Checklist
✅ Node.js and npm installed

✅ Backend URL updated in TodoApp.js

✅ Frontend dependencies installed via npm install

✅ Project built via npm run build

✅ Build files copied to /var/www/html

✅ NGINX configured and restarted

✅ Optional: Private IP reverse proxy configured

✅ App accessible via browser

📌 Notes
Ensure inbound ports 80 (frontend) and 8000 (backend) are open in firewall/security groups.

Use curl or browser to validate access to /api and app interface.

For production environments, consider using SSL (HTTPS) and domain-based routing.