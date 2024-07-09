# Deployment_Of_ReactApp_to_AWS_Elastic_Beanstalk
## Step 1: Clone the source code
If you are using Ubuntu Machine, Git will be pre-installed. Clone the repository by using 'git clone' command and move into there.

We shall only work in the same directory for the duration of this project. The reason you will understand at last stage.
```
git clone https://github.com/neelshah2409/Deployment_Of_ReactApp_to_AWS_Elastic_Beanstalk.git
cd Deployment_Of_ReactApp_to_AWS_Elastic_Beanstalk
```
## Step 2 : Install docker
After cloning the code, do ll or ls and you will find a shell-script called docker_install.sh to install and enable docker for you. To run the code, you must first make it executable.

Run the commands listed below, then wait until Docker is installed and running for you automatically. 😁
```
chmod +x docker_install.sh
sh docker_install.sh
```
(Try sudo if you face any error)

## Step 3 : Create a Multi-Stage Dockerfile for our application
Now, let's understand the requirement first. We require node to be installed and running in the background in order to run the react application. Also, we will need nginx to serve the requests which will help us to access the application after deploying in EB.

Let's create a multi-stage docker file accordingly.

Write a Dockerfile with the following code:
```
FROM node:14-alpine as builder
WORKDIR /app 
COPY package.json . 
RUN npm install 
COPY . . 
RUN npm run build

FROM nginx 
EXPOSE 80 
COPY --from=builder /app/build /usr/share/nginx/html
```
Explanation :
This Dockerfile has two stages. The first stage builds a web application with NPM using the official Node.js 14 Alpine image. The second stage serves the constructed web application on port 80 using the official NGINX image. The built application files from the first stage are copied to the second stage, which is the final image, that can be used to run the web application in a Docker container.

## Step 4: Building Docker Image
Now it's time to build Docker Image from this Dockerfile. To create the Docker image, run the command below.

```
sudo docker build -t <provide_image_name> .
```
## Step 5: Running Docker Container

```
sudo docker run -d -p 80:80 <image_name>
```
Check running docker container with docker ps command.

Utilizing ec2 public_ip on port 80, you can also confirm that the application is active.

## Step 6 : Configure Elastic BeanStalk
- Go to the AWS Elastic BeanStalk Service.
- Click on `Create Application`
- Provide the details such as Name = "<Any name>", Platform = "Docker", and select "Sample Code". Sample code is nothing but a basic 
  code, provided by AWS, which is a very basic application to test our flow; and later we can modify it accordingly and give our code.
- Now Click on `Create Application` .
- You can see that it's also establishing an environment for the application in the environment section.


All right !! 🎉 Your environment is Up now. The sample application is deployed on Docker.
AWS EB will continuously monitor our environment and show us the status. As of now, it's HEALTHY.
To access the sample application, click on the link shown above the "Application name" .
It should look like this.

![image](https://github.com/neelshah2409/Deployment_Of_ReactApp_to_AWS_Elastic_Beanstalk/assets/71593494/53ed682b-9e81-4e80-b63a-a73f7cb90396)

