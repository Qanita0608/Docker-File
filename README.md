MongoDB and Mongo Express Containers on AWS EC2

This project demonstrates the deployment of MongoDB and Mongo Express containers using Docker on an AWS EC2 instance. Additionally, it showcases the deployment of a two-tier web application using Docker Compose.

Prerequisites

- AWS account
- EC2 instance with Amazon Linux 2
- SSH access to the EC2 instance
- Docker installed on the EC2 instance

Setup Instructions:

1. Launch an EC2 Instance
Log in to your AWS account and navigate to the EC2 dashboard.
Launch a new instance using the Amazon Linux 2 AMI.
Configure security groups to allow traffic on the following ports:
   - `22` for SSH access
   - `8081` for Mongo Express
   - `3000` for the web application

2. Install Docker
Connect to your EC2 instance via SSH and run the commands to install Docker:

Code:
sudo yum update -y

sudo amazon-linux-extras install docker

sudo service docker start

sudo usermod -a -G docker ec2-user

3. Create a Custom Docker Network
Run the following command to create a custom Docker network:
Code:
docker network create mongo-network

4. Run MongoDB and Mongo Express Containers
Run the following commands to start the MongoDB and Mongo Express containers:

MongoDB Container:
Code:
docker run -d --name mongodb --network mongo-network \
-e MONGO_INITDB_ROOT_USERNAME=root \
-e MONGO_INITDB_ROOT_PASSWORD=password \
mongo:latest

Mongo Express Container:
Code:
docker run -d --name mongoexpress --network mongo-network \
-p 8081:8081 \
-e ME_CONFIG_MONGODB_SERVER=mongodb \
-e ME_CONFIG_MONGODB_ADMINUSERNAME=root \
-e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
mongo-express:latest

6. Deploy a Two-Tier Web Application Using Docker Compose
Create a directory for your application:
Code:
mkdir myapp
cd myapp
Create a docker-compose.yml file with the following content:
yaml
Code:
version: '3.8'
services:
  web:
    image: node:14
    container_name: web
    working_dir: /app
    volumes:
      - .:/app
    ports:
      - "3000:3000"
    command: sh -c "npm install && npm start"
    depends_on:
      - db

  db:
    image: mongo:latest
    container_name: mongodb
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: example
    volumes:
      - mongodata:/data/db
    ports:
      - "27017:27017"

volumes:
  mongodata:

Start the application using Docker Compose:

Code:
docker-compose up -d

Verify the deployment by accessing the web application via your EC2 instance's public IP on port 3000 and Mongo Express on port 8081.

Conclusion:
This project illustrates how to use Docker for containerizing applications and AWS EC2 for cloud deployment. The combination of these tools provides a powerful solution for scalable and efficient application management.

These explanations should provide a clear understanding of your project, tools, and processes, and guide others on how to replicate your setup. Let me know if you need further assistance or adjustments.
