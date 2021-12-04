## _Instructions on how to prepare an application image,infrastructure and then deploy this image with Docker and Kubernetes_


**Docker** is a set of platform as a service products that use OS-level virtualization to deliver software in packages called containers.  
**Kubernetes** (commonly stylized as K8s) is an open-source container-orchestration system for automating computer application deployment, scaling, and management.
1.	Download Docker [here](https://docs.docker.com/get-docker/);
2.	Docker Installation instructions [here](https://docs.docker.com/get-started/);
3.	Download Kubernetes [here](https://kubernetes.io/releases/download/);
4.	To run the container with the application in the Kubernetes cluster:  make sure ExampleApp accepts requests on port 8800;
5.	Create a directory and subdirectories:
to open console in Linux use Ctrl+Alt+F1;
to open console in Windows 8, 8.1, 10 press  Win+S,type cmd in the search bar;
to run the command line:
with the administrator rights, right-click on the Command line and select Run as Administrator;
with the rights of a regular user, select Command line.
To open console in Windows 7 press Win+R.

DO NOT CLOSE THE CONSOLE DURING THE WHOLE PROCESS!

6.	In the console choose the directory you would like to work in:
Type cd followed by a space in the command prompt window.
Drag and drop the folder you want to browse into the window.
Press Enter.

7.	Type:
```sh
mkdir quickstart_docker
mkdir quickstart_docker/application
mkdir quickstart_docker/docker
mkdir quickstart_docker/docker/application
```
 The structure:
quickstart_docker/ # Catalog of the entire project
├──application/      # Project code
└──docker/               # Docker stuff
   └──application/    # Dockerfile(a text document that contains all the commands a user could call on the command line to assemble an image) for ExampleApp

8.	For the example deployment  put application.py in the quickstart_docker/application directory in the console type:
```sh 
import http.server
import socketserver
PORT = 8000
Handler = http.server.SimpleHTTPRequestHandler
httpd = socketserver.TCPServer(("", PORT), Handler)
print("serving at port", PORT)
httpd.serve_forever()
```
9.	The application needs an environment. At a minimum, python will be needed and  OS with all the dependencies.You can find it [here](https://hub.docker.com/_/python) 
Python and its dependencies have to be downloaded in the same directory as Docker.

10.	Still in console in the quickstart_docker/docker/application directory put a file named Dockerfile and the following contents:
```sh
# Use base image from the registry
FROM python:3.5

# Set the working directory to /app
WORKDIR /app

# Copy the 'application' directory contents into the container at /app
COPY ./application /app

# Make port 8000 available to the world outside this container
EXPOSE 8000

# Execute 'python /app/application.py' when container launches
CMD ["python", "/app/application.py"]
```
11.	In console type:
```sh
docker build . -f-docker/application/Dockerfile -t exampleapp
```
**Arguments**: 
.- working directory, build context;
-f docker/application/Dockerfile - docker-file; 
-t exampleapp - tag the image so that you can find it later.
Learn more about building images for Docker [here](https://docs.docker.com/engine/reference/builder/)

12.	To look through the list of images in the console type "$ docker images" :

| REPOSITORY|TAG  | IMAGE ID | CREATED | SIZE| 
| ------ | ------ | ------ | ------ | ------ |
|exampleapp  |           latest   |        83wse0edc28a   |    2 seconds ago    |    153MB|
|python      |           3.6      |       05sob8636w3f    |    6 weeks ago      |   153MB|

13.	Upload the image to the repository:[here](https://www.cloudbees.com/blog/using-docker-push-to-publish-images-to-dockerhub)
