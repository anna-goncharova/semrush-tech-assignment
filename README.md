# How to Deploy a Python Application on Kubernetes

This guide provides a detailed walkthrough for containerizing a basic Python application and deploying it to a Kubernetes cluster. We will cover the process of setting up the required directories and files, building the Docker image, and deploying the application within a Kubernetes environment.


## Prerequisites

Ensure you have the following tools installed:

* Docker: [get Docker](https://docs.docker.com/get-docker/);
* Kubernetes: [install Kubernetes](https://kubernetes.io/docs/setup/) (with `kubectl` configured).


## Step 1: Set Up the Project Structure

Start by creating the project directory structure. Open your terminal and run the following commands:

```bash
mkdir -p quickstart_docker/application
mkdir -p quickstart_docker/docker/application
```

Your directory structure should be the following:

```
quickstart_docker/      # Catalog of all project
├── application/        # Application code
└── docker/
    └── application/    # Dockerfile and related Docker configurations
```


## Step 2: Create the Python Application

Create a simple Python application. Navigate to the `quickstart_docker/application` directory and create a file named `application.py`:

```python
import http.server
import socketserver

PORT = 8000
Handler = http.server.SimpleHTTPRequestHandler

httpd = socketserver.TCPServer(("", PORT), Handler)
print("serving at port", PORT)
httpd.serve_forever()
```

This script starts an `HTTP` server that listens on port `8000`.


## Step 3: Create the Dockerfile

Create a Dockerfile to containerize the application. Go to the `quickstart_docker/docker/application` directory and create a file named `Dockerfile`:

```dockerfile
# Use the official Python 3.5 image from Docker Hub
FROM python:3.5

# Set the working directory inside the container to /app
WORKDIR /app

# Copy the application code into the container
COPY ./application /app

# Expose port 8000 to be accessible from outside the container
EXPOSE 8000

# Run the application
CMD ["python", "/app/application.py"]
```


## Step 4: Build the Docker Image

In your terminal navigate to the root of the `quickstart_docker` directory and run:

```bash
docker build -f docker/application/Dockerfile -t exampleapp .
```

Explanation of the command:

*  `-f docker/application/Dockerfile`: points to the Dockerfile location;
*  `-t exampleapp`: tags the image as `exampleapp`;
* `.`: specifies the current directory as the build context.

After building, verify the image was created:

```bash
docker images
```


# Conclusion

You've successfully containerized a Python application using Docker and deployed it to a Kubernetes cluster. For more information, refer to the official [Docker documentation](https://docs.docker.com/) and [Kubernetes documentation](https://kubernetes.io/docs/).
