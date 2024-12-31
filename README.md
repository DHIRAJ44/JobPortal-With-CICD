Project Overview

This project demonstrates a continuous integration and continuous deployment (CI/CD) pipeline for a MERN stack job portal application. The pipeline automates the process of pulling code changes from GitHub, building the application using Docker, deploying it on AWS EC2, and reflecting live updates on the website. Notifications are sent via email for successful and failed builds.



Project Architecture

The architecture consists of:

Source Code Repository: GitHub hosts the MERN stack job portal code.

CI/CD Tool: Jenkins automates the build, test, and deployment process.

Containerization Tool: Docker ensures consistent deployment across environments.

Cloud Infrastructure: AWS EC2 hosts the application.

Tools and Technologies

GitHub: Version control and source code management.

Jenkins: Automation server for CI/CD.

Docker: Containerization tool for packaging the application.

AWS EC2: Cloud service for hosting the application.

Nginx: Reverse proxy server.

Node.js & Express: Backend framework for the job portal.

React.js: Frontend framework for the job portal.

MongoDB: Database for storing application data.

Implementation Steps

Step 1: Setup Jenkins on AWS EC2

Launch an EC2 instance with appropriate resources.

Install Jenkins:

sudo apt update
sudo apt install openjdk-11-jdk -y
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y

Configure Jenkins and install required plugins (e.g., Git, Docker, Email Extension).

Step 2: Configure GitHub Repository

Push the MERN stack job portal code to GitHub.

Set up a webhook in the repository to trigger Jenkins jobs on code updates.

Step 3: Automate Deployment with Scripts

Write a shell script to automate the deployment process:

#!/bin/bash

# Step 1: Pull the latest code from GitHub link
https://github.com/DHIRAJ44/JobPortal-With-CICD

# Step 2: Build Docker image
docker build -t job-portal:latest ./job-portal

# Step 3: Stop and remove existing container if running
docker stop job-portal || true
docker rm job-portal || true

# Step 4: Run the new container
docker run -d -p 80:3000 --name job-portal job-portal:latest

# Step 5: Cleanup old images (optional)
docker image prune -f

Save the script on the Jenkins server (e.g., /home/ubuntu/deploy.sh).

Make the script executable:

chmod +x /home/ubuntu/deploy.sh

Create a Jenkins freestyle job to execute the script:

In the build step, add: Execute shell and specify the script path: /home/ubuntu/deploy.sh.

Step 4: Configure Email Notifications

Install the Email Extension plugin in Jenkins.

Configure SMTP settings under Manage Jenkins > Configure System > E-mail Notification.

Step 5: Test the Automation

Push changes to the GitHub repository.

Observe Jenkins triggering the job.

Verify live updates on the deployed website.

Check for email notifications upon build completion or failure.

Flow Diagram

GitHub Repository --> Jenkins Job --> Shell Script --> Docker Build --> EC2 Deployment --> Live Website
                                                         |
                                                Email Notification

Configurations and Code Samples

Dockerfile

FROM node:14
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]

Automation Script

Refer to the script in Step 3.

Email Notifications

Email is configured to notify the development team of build statuses.

Uses Jenkins Email Extension plugin.

Key Features

Fully automated CI/CD pipeline.

Seamless integration with GitHub for live code updates.

Dockerized deployment for consistency.

Hosted on AWS EC2 for scalability and reliability.

Real-time email notifications for build status.

Conclusion

This CI/CD pipeline ensures efficient, automated deployment of the MERN stack job portal application. By leveraging Jenkins, Docker, and AWS EC2, the process reduces manual effort, improves reliability, and enhances the development workflow.
