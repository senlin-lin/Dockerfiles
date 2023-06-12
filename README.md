# Dockerfiles

## 1. Create a new Docker Image and Push to Docker Hub

### 1.1 Building a new Docker Image on your local machine

If you'd like to use the provided image directly, you can skip to step 2. Otherwise, build a Docker image in the directory containing your modified Dockerfile using the following command:

```bash
docker build -t my_custom_image .

This command will use the Dockerfile to create a Docker image named my_custom_image.

### 1.2 Pushing your Docker Image to Docker Hub or other Docker image repositories

On your local machine, log into Docker Hub or the Docker image repository of your choice. For example, if you choose Docker Hub, use the following command to log in:

```bash
docker login

You'll need to create a Docker Hub account in advance and enter your username and password.

### 1.3 Push your Docker Image to Docker Hub

Replace <username> with your Docker Hub username:

```bash
docker tag my_custom_image:latest <username>/my_custom_image:latest
docker push <username>/my_custom_image:latest

## 2. Deploy the Docker Image on a new Server

### 2.1 Install Docker on the new server

The installation process may vary depending on the operating system. Here are the basic commands to install Docker on Ubuntu:

```bash
sudo apt-get update
sudo apt-get install docker-ce

### 2.2 Pull your image from Docker Hub

```bash
docker pull <username>/my_custom_image:latest

## 3. Run the Docker container on your new server

Now, you can run your Docker container on the new server. The following command will run your container and map port 8888 of the server to port 8888 of the container:

```bash
docker run -p 8888:8888 <username>/my_custom_image:latest

If your Docker configuration is GPU version and you want to use GPU, you need to run the following command:

```bash
sudo docker run --gpus all -p 8888:8888 <username>/my_custom_image:latest

## 4. Access Jupyter Notebook

After running the container on your new server, you should be able to use Jupyter Notebook by accessing port 8888 on your server. If your server's IP address is 'server_ip', you can enter the following address into your web browser:

`http://server_ip:8888`








