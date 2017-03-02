# Docker Containers 101 - Hands-On-Lab for Beginners - Participant Guide

### Table of Contents:

1. [What is a Container?  What is Docker?](https://gist.github.com/mikeraab/6a8c64ce3ebb81f4f8c886209a78e8f1#what-is-a-container-what-is-docker)

2. [Verify Docker Engine Install](https://gist.github.com/mikeraab/6a8c64ce3ebb81f4f8c886209a78e8f1#verify-docker-engine-install)

3. [Run Your First Container](https://gist.github.com/mikeraab/6a8c64ce3ebb81f4f8c886209a78e8f1#run-your-first-container)

4. [Docker Images and Docker Hub](https://gist.github.com/mikeraab/6a8c64ce3ebb81f4f8c886209a78e8f1#docker-images-and-the-docker-hub)

5. [Stop and Re-run Your Container with a More Descriptive Name](https://gist.github.com/mikeraab/6a8c64ce3ebb81f4f8c886209a78e8f1#stop-and-re-run-your-container-with-a-more-descriptive-name)

6. [Build Your Own Image](https://gist.github.com/mikeraab/6a8c64ce3ebb81f4f8c886209a78e8f1#build-your-own-image)

7. [Registries](https://gist.github.com/mikeraab/6a8c64ce3ebb81f4f8c886209a78e8f1#registries)

8. [Introduction to Docker Compose](https://gist.github.com/mikeraab/6a8c64ce3ebb81f4f8c886209a78e8f1#introduction-to-docker-compose)

9. [Understanding Basic Docker Networking](https://gist.github.com/mikeraab/6a8c64ce3ebb81f4f8c886209a78e8f1#understanding-basic-docker-networking)

10. [Understand Docker Volumes](https://gist.github.com/mikeraab/6a8c64ce3ebb81f4f8c886209a78e8f1#understand-docker-volumes)

11. [Running Applications Across Multiple Docker Hosts](https://gist.github.com/mikeraab/6a8c64ce3ebb81f4f8c886209a78e8f1#running-applications-across-multiple-docker-hosts)

12. [CI/CD Integration with GitHub, DockerHub and OCCS](https://gist.github.com/mikeraab/6a8c64ce3ebb81f4f8c886209a78e8f1#cicd-integration-with-github-dockerhub-and-occs)

13. [Summary and More Resources](https://gist.github.com/mikeraab/6a8c64ce3ebb81f4f8c886209a78e8f1#summaryrecap-pointer-to-further-resources)

### Summary - This hands-on-lab in targeted at beginners with no prior experience and will cover the basics of using Docker.  Participants will advance concepts and skills from Docker using CLI on a Docker Host/VM to higher level tools, such as Github, Docker Hub and Oracle Container Cloud Service (OCCS).

### Pre-requisites:

* Access to the Internet

* An Oracle Cloud Trial

* A pre-provisioned instance of Oracle Container Cloud Service

    * [see Youtube to watch a short video for setup and access](https://youtu.be/fusfnOLBjBc) 

* A laptop

* A SSH client

* The SSH key used to create the Container Cloud Service instance

* A text editor to keep notes and snippets as you go along

* A Docker Hub account

* A Github account

## What is a Container? What is Docker?

Basic intro to containers and Docker.  Brief history of Linux Containers.  Basic Differences with Virtualization.

A container is a runtime instance of a docker image.

[https://docs.docker.com/engine/reference/glossary/#/container](https://docs.docker.com/engine/reference/glossary/#/container)

Docker is the company and containerization technology.

[https://docs.docker.com/engine/reference/glossary/#/docker](https://docs.docker.com/engine/reference/glossary/#/docker)  

Containers have been around for many years.  Docker created a technology that was usable by mere humans, and was much easier to understand than before.  Thus, has enjoyed a tremendous amount of support for creating a technology for packaging applications to be **portable and lightweight.**

History of Linux Containers

<img src=images/001-container-history.jpg />
***

VM vs Container

<img src=images/002-vm-vs-container.png />
***
While containers may sound like a virtual machine (VM), the two are distinct technologies. With VMs each virtual machine includes the application, the necessary binaries and libraries and the **entire guest operating system**

Whereas, Containers include the application, all of its dependencies, but share the kernel with other containers and are not tied to any specific infrastructure, other than having the Docker engine installed on it’s host – containers run on any computer, infrastructure* and cloud.  

> *Please note that at this time, Windows and Linux containers require that they run on their respective kernel base, therefore, Windows containers cannot run on Linux hosts and vice versa*

## Verify Docker Engine Install

The lab will leverage a pre-built Oracle Container Cloud Service (OCCS) instance for convenience and to utilize its UI to verify some of the exercises.  You should have already provisioned your OCCS instance as part of the prerequisites for this workshop.  

The first step will be to log in to one of the OCCS "worker nodes", or Docker host, and verify the Docker installation and check the version.  A worker node is simply a Docker Host/VM that can run Docker containers.

First SSH into a Worker Node in your Pre-built ContainerCS instance using the SSH key that you used when you provisioned the OCCS instance. 

To find a Worker Node IP address, log into your Oracle Cloud My Services Portal and use one of the Public IPs from a Worker Note in the Container Cloud Service Console:

<img src=images/003-worker-ip.png />
***
Modify the below command with your Worker node IP and the path for your private key.

```
$ ssh opc@ip_address -i /users/yourName/folder/sshkey/privateKey
```

> *Note, the above format is usable directly in a Mac terminal. If you are using a Windows computer, use an appropriate Windows SSH client, like Putty or another SSH client*

For convenience, run all commands as root

```
$ sudo -s
```

Ensure that the Docker engine is running

```
$ service docker status
```

Check the version of Docker engine

```
$ docker version
```

## Run Your First Container

Run Docker’s Helloworld example

	
```
$ docker run hello-world
```

Since the "hello-world" image is not available locally on the host, the command automatically pulls the hello-world image from the public Docker Hub and runs the container in the foreground.

Congratulations, you have just run your first Docker container!

List all containers (- a = running and stopped)

```
$ docker ps -a
```

## Docker Images and the Docker Hub

**Browse to a public image on Docker Hub.  Run it.**

Open a browser and go to this URL:

[https://hub.docker.com/r/karthequian/helloworld](https://hub.docker.com/r/karthequian/helloworld)

Pull the image from the Docker Hub Registry - Note how the layers are pulled individually

```
$ docker pull karthequian/helloworld:latest
```

Copy/Paste the Docker Run command from the Docker Hub page and add a -d option so the container runs in "detached" mode (as opposed to the foreground).  This frees up your terminal window.

```
$ docker run -d -p 80:80/tcp "karthequian/helloworld:latest"
```

Explore the Helloworld app in the browser.  Navigate to the IP of the Docker Host where it is running and note the number of visits.  (The IP is the same as the Host that you are SSH’d into):
http://host_ip

<img src=images/004-hello-world.png />
***
You are now actually using an application that is in the Docker container.  Refresh the browser and observe how the visits counts increments.  This is a live application. A simple example, but an example of the experience of using an application running in a container, which is no different than if it was not running in a container.

> *Makes you wonder about how many apps that you are using on a day to day basis, may indeed be running in a Docker container?*

Now, let’s see the same Container in OCCS UI

You will need to log into your instance of Container Cloud Service

Find the login on the Container Cloud Services Console page in your Oracle Public Cloud trial

<img src=images/005-occs-access.png />
***
Log into the OCCS with the credentials used when you created the instance

<img src=images/006-occs-login.png />
***
Navigate via the Left Hand nav to the Containers page and find the container that is running the Helloworld app.

If you have arrived at a page that looks like the below, you will have arrived at the right page.

<img src=images/007-stoic-wilson.png />
***
Notice that Docker has assigned a container name "stoic_wilson" in the above?  What name did Docker give your container?  Remember this name, as we will use it in a bit.

> *Note, unless you specify a container name, Docker will assign a similar 2 part name automatically*

## Stop and Re-run Your Container with a More Descriptive Name

Now, let's go back to the terminal window, stop the container and give it a more descriptive name, so that we could find it easier if there were many containers running.

Stop the Running Container - Replace your_container below with an actual name that you want to call your running container

```
$ docker stop your_container
```

Now, remove the container with the "rm" command

```
$ docker rm your_container
```

Check to be sure that the container has been removed

```
$ docker ps -a
```

> *Note, containers can be stopped and removed by using their name **(if there are no dependent image layers)**, their long id or their short id*

Now run the container with a more descriptive name, such as "helloworld_app":

```
$ docker run -d --name helloworld_app -p 80:80/tcp "karthequian/helloworld:latest"
```

Revisit the Container Cloud Service UI.  

> *Is the container easier to find now, especially that there is context to the name of the container?*

> *Notice how you can Stop and Remove the container, directly from the UI*

Feel free to Stop and Remove the container.  We are done with this part of the HOL.
***
<img src=images/008-stop-container.png />
***
## Build Your Own Image

About DockerFiles

A Dockerfile is a recipe that starts with a base image (typically a thin Linux OS distribution such as Alpine Linux), and then layers on an app and configuration.  [According to Docker](https://docs.docker.com/engine/reference/builder/): 

*A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using docker build users can create an automated build that executes several command-line instructions in succession.*

**Build the Docker image**

Now, let’s use the [Docker Whale](https://docs.docker.com/engine/getstarted/step_three/) example to build our first image.  

Follow Steps 1 and 2 from this exercise:

[https://docs.docker.com/engine/getstarted/step_four/](https://docs.docker.com/engine/getstarted/step_four/)

Here is a synopsis of the steps in the above URL:

Make a directory to store your Dockerfile

```
$ mkdir mydockerbuild
```

Change to the new directory

```
$ cd mydockerbuild
```

In Step 1.3, use VI (or editor of your choice) instead of nano

```
$ vi Dockerfile
```

Create a text file name Dockerfile with these 3 lines:

```
FROM docker/whalesay:latest

RUN apt-get -y update && apt-get install -y fortunes

CMD /usr/games/fortune -a | cowsay
```

In Step 1.8, after you are done adding the 3 lines to your Dockerfile with VI, save the file by typing the Esc key - colon - w (for write) - q (for quit):

	
```
esc : w q 
```

> *Note, docs for VI are here: [https://www.cs.colostate.edu/helpdocs/vi.html](https://www.cs.colostate.edu/helpdocs/vi.html)*

Then per section 2, build your Docker image, be sure to include the period at the end of the command

```
$ docker build -t docker-whale .
```

Then per section 4, list the images on your host and run the docker-whale image as a container

```
$ docker images
```

```
$ docker run docker-whale
```

Notice the output in the terminal.  Re-run the image a couple of times, as the container will run once, then stop.

By contrast, navigate to one of the stopped docker-whale containers in your OCCS instance and select the "View Logs" button to easily view the container logs 

<img src=images/009-container-logs.png />
***

## Registries

Registries store Docker images.  Using a registry is the first step towards moving Docker off the laptop.  The most widely used registry is the Docker Hub.  [https://hub.docker.com](https://hub.docker.com) 

> *Note, in this exercise you will need a Docker Hub account.  If you do not have one already, you can signup for free, navigate to: [https://hub.docker.com*/](https://hub.docker.com/)*

**Tag and Push your new image to the Docker Hub registry.  In this exercise username will be your Docker Hub account name.**

First, log into your Docker Hub account from the terminal:

```
$ docker login
```

When prompted, enter your Docker account username (lowercase), password and email.

Now, tag and push your new docker-whale image to your account on Docker Hub

Substitute your Docker username below:

```
$ docker tag docker-whale:latest username/docker-whale:latest
```

Push the Docker image to your account.  This will create a new repository called "docker-whale" for this image.

```
$ docker push username/docker-whale:latest
```

Navigate to your account page in Docker Hub via this URL, substituting your username:

[https://hub/docker.com/r/username](https://hub.docker.com/r/username)

Do you see the image that you pushed?

<img src=images/010-docker-hub.png />
***
Now, remove the local image and run the image from the registry

To do this, you must first remove the stopped container by using its short id, not its name.  Find the short id.

```
$ docker ps -a 
```

Copy the short id for the appropriate container, it will be similar to this format: ee31fe1dd8f8 and use the "rm" command to remove the container 

```
$ docker rm short_id
```

Now that the container is removed, you can remove the image and force the container to be run from the image on the Docker Hub with the "rmi" command

Remove the image that you pushed to the Docker Hub

```
$ Docker rmi username/docker-whale
```

Verify the images are removed.  View all Docker images with this command:

```
$ docker images
```

Now, run the image directly from your repository on Dockerhub, and force a new pull of the image (because the image does not exist locally)

```
$ docker run username/docker-whale
```

> *Note, if no tag is used, the default tag is "latest"*

**Add the registry to OCCS and run your image there**

On the OCCS Registries Page, create and save a new Registry definition using your Docker hub account, email, username and password.  You can also add a description.

> *Note that the URL for your Docker Hub repo/account should be in this format: index.docker.io/username*

> *Clicking the Validate button will show a green banner, indicating a successful Docker Login to the account*


<img src=images/011-add-registry.png />
***

Create a new Service with your Docker Run command

```
Left nav - Services > New Service Button
```

Copy and Paste this docker run command into the Docker Run tab in the new Service.  Be sure to substitute your Docker username to pull from your repo on Docker Hub.

```
docker run \
  -e="OCCS_REMOVE_ON_DIE=0" \
  "username/docker-whale"
  ```
***
<img src=images/012-new-service.png />
***
Enter a name for the new Service, such as "whale" and Save the new Service

To run the Whale service, just click the Deploy Button next to Whale service, in the list of available Services, then click Deploy again and accept the default orchestration options.

<img src=images/013-deploy-service.png />
***

The Service will be deployed, run and stop, just like when we ran in the terminal.

> *Tip, normally, Deployments would be self-healing and restart a stopped container.  In this case, we choose not to automatically restart the container with the addition of a one- time run environment variable: -e="OCCS_REMOVE_ON_DIE=0" \*

Click on the container name

<img src=images/014-select-container.png />
***

Then click the "View Logs" button to see the whale’s comments

<img src=images/015-view-whale-comments.png />
***

## Introduction to Docker Compose

What is Docker Compose, why use it?

According to Docker: Docker Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a Compose file to configure your application’s services. Then, using a single command, you create and start all the services from your configuration.

Let's explore this further

Install Docker compose

> *Note, docs are here: [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)*

Use these specific below commands in your terminal for this exercise to install in your home directory.

```
$ curl -L "https://github.com/docker/compose/releases/download/1.10.0/docker-compose-$(uname -s)-$(uname -m)" -o /home/opc/docker-compose
```

Change the executable permissions

```
$ chmod +x /home/opc/docker-compose
```

Verify and check which version of Docker Compose was installed

```
$ ./docker-compose --version
```

Follow these steps to create a simple Wordpress stack referenced here:

[https://docs.docker.com/compose/wordpress/](https://docs.docker.com/compose/wordpress/) 

Here is a synopsis of the steps in the above URL:

Use VI to create a file named docker-compose.yml that contains the following text:

```
version: '2'
services:
   db:
 	image: mysql:5.7
 	volumes:
   	- db_data:/var/lib/mysql
 	restart: always
 	environment:
   	MYSQL_ROOT_PASSWORD: wordpress
   	MYSQL_DATABASE: wordpress
   	MYSQL_USER: wordpress
   	MYSQL_PASSWORD: wordpress
   wordpress:
 	depends_on:
   	- db
 	image: wordpress:latest
 	ports:
   	- "8000:80"
 	restart: always
 	environment:
   	WORDPRESS_DB_HOST: db:3306
   	WORDPRESS_DB_PASSWORD: wordpress
volumes:
	Db_data:
```

Run the Wordpress stack by this command

```
$ ./docker-compose up -d
```

Verify the running stack, by visiting the Wordpress setup page

In your browser, navigate to the IP of the Docker host, port 8000

```
http://docker_host_ip:8000/wp-admin/install.php
```

Congratulations, you have successfully launched your first Wordpress app in Docker!

Stop and Remove the running Wordpress and Database containers by using their short id

Remember this from a previous exercise:

```
docker ps -a

docker stop short_id

docker rm short_id
```

Repeat for next container

## Understanding Basic Docker Networking

The objective of this section is to understand port mappings and dynamic port allocations

**For the remainder of the HOL, we will be using the Container Cloud Service**

Log into your Container Cloud Service instance

Navigate to the Hello World Application example service on the Services page

Click the edit button next to the Hello World service and scroll down to the port settings

<img src=images/016-edit-helloworld.png />
***

Notice that the default host port setting is 9000, which means that the container port 80 is mapped to the host port 9000.  If this service is deployed (docker run), host port 9000 on the particular Worker node would be consumed, and no more of this same container could run on that host as it is currently configured.

To get past this limitation, Docker has a feature that allows for containers to be assigned a dynamic port in the 32000 range.  Because of this feature, you can then run multiple containers on the same host, that would otherwise contend with each other for the host port.

To modify the service to use dynamic ports, edit the port and remove port 9000 from the host and leave it blank.

<img src=images/017-remove-port.png />
***

Save the port edit and use "Save As" to create a new Service called Hello World Dynamic.  Save the new service.

<img src=images/018-save-as-dyn.png />
***

Now deploy the new Hello World Dynamic service and add 3 for the Quantity of containers and set a "Constraint" to deploy this on only one of your hosts, as per the below screenshot.  Once the options are set, click deploy.

<img src=images/019-deploy-hw-dyn.png />
***

The deployment will automatically run 3 Hello World containers, each running on different dynamic ports on the same Docker Host.

<img src=images/020-hw-hosts.png />
***

**Now, use your browser to navigate to the IP and Port of one of the containers.**

To find the IP of the Host, click on the host under Hostname

**GET Image**
<img src=images/020a-host-ip.png />
***

From this screen, copy the Public IP address shown here
<img src=images/021-host-ip.png />
***

Now, click on one of the 3 running Hello World containers running on this host
<img src=images/022-dyn-container.png />
***

Note the Host port, in this case 32771 in the below screenshot
<img src=images/023-container-port.png />
***


Now, navigate with your browser to that IP and Host Port
<img src=images/024-dyn-port-hw.png />
***


You can then do the same for each of the 3 containers.  This Docker feature provides a valuable advantage of **running multiple copies of the same application on the same host**, increasing efficiency.

## Understand Docker Volumes

This section will explore data persistence through the use of host data volumes

A full introduction to Docker volumes is located here: [https://docs.docker.com/engine/tutorials/dockervolumes/](https://docs.docker.com/engine/tutorials/dockervolumes/)

In short, unless a container volume is mounted to a persistent host volume, any data stored within the container will be lost when the container is removed.

So let's explore how you might persist data with a Wordpress stack within Container Cloud Service

In your OCCS instance, go to the Stacks page.  Here you will see 2 example Wordpress stacks.

**get image**
<img src=images/025-new-stack.png />
***

But lets create a new Wordpress stack from the below YAML that has some included volume statements.  Click the New Stack Button and then Advanced Editor.

<img src=images/026-advanced-editor.png />
***

Paste this YAML into the Advanced Editor

```
version: 2
services:
  wordpress:
	image: "wordpress:4.5.2"
	ports:
  	- 80:80/tcp
	environment:
  	- OCCS_PHASE_ID=1
  	- "WORDPRESS_DB_HOST={{ proxy \"db:3306\" }}"
  	- WORDPRESS_DB_PASSWORD=example
  	- MYSQL_ROOT_PASSWORD=example
  	- "OCCS_HEALTHCHECK_WORDPRESS_HTTP=tcp://:80/?timeout=10s&interval=30s"
  	- "occs:description=This is a simple wordpress stack that can be deployed to multiple hosts. This example is provided as-is for educational purposes and should not be used in production.  This example also has persistent volumes"
	volumes:
  	- "/var/www/html:/var/www/html:rw"
  db:
	image: "mariadb:10.1.14"
	ports:
  	- 3306/tcp
	environment:
  	- OCCS_PHASE_ID=0
  	- MYSQL_ROOT_PASSWORD=example
  	- "OCCS_HEALTHCHECK_MYSQL=tcp://:3306/?timeout=10s&interval=30s"
	volumes:
  	- "/var/lib/mysql:/var/lib/mysql:rw"
```

The YAML above has the addition of volumes configured for both the Wordpress and the Database highlighted in yellow, that will be mounted to the host volume where these containers are running to persist data from the database, blog images and Wordpress themes.

Notice that the volume in the container is listed first. Then the host volume, with the option rw for read and write. (as opposed to ro, for read only).

<img src=images/027-paste-yaml.png />
***

Save the Advanced Editor and then give the stack a name, Wordpress persistent.  Then save the new stack.

<img src=images/028-save-wp-stack.png />
***

Once saved, you will see it listed and you can deploy this new Stack

**CHECK IMAGE**
<img src=images/029-deploy-wp.png />
***

Now Deploy the Wordpress persistent stack

<img src=images/029-deploy-wp.png />
***

When the Wordpress stack is deployed and running.  Make a note of the host locations for both the Wordpress and Database containers.  You will need these later when we re-deploy the stack so the containers can persist their data on the same host for each container.

<img src=images/030-wp-hosts.png />
***

Find the IP of the Wordpress container.  Click the Hostname for the Wordpress container.

<img src=images/031-wp-host-ip.png />
***

Copy the IP address

<img src=images/032-wp-host-ip.png />
***

In your browser navigate to Host_IP and append it with the Wordpress initialization URL: /wp-admin/install.php.  

> *Note, this is the same setup URL you saw when we deployed with Docker Compose, however this time, we are going to setup Wordpress and create blog post.

[http://ip_address/wp-admin/install.php](http://ip_address/wp-admin/install.php)

First, select your language

<img src=images/033-wp-setup1.png />
***

Setup the Wordpress login details.  Be sure to keep the Username and Password in your notes.

<img src=images/034-wp-setup2.png />
***

Click "login" to log into to Wordpress

<img src=images/034-wp-setup2.png />
***

Login using the credentials you created earlier

<img src=images/036-wp-login2.png />
***

Select "Write your first blog post" in the Next Steps section

<img src=images/037-write-blog.png />
***

Create a sample blog post.  Include an image of your choosing, if you would like and click Publish.

<img src=images/038-publish-blog.png />
***

Click on the "Permalink" to navigate to the blog post

<img src=images/039-permalink.png />
***

Copy the URL of the blog post and keep this in your notes, as you will need it later

<img src=images/040-view-blog.png />
***

Back in OCCS, use the "Stop" button to stope the Wordpress deployment, which will stop each container in an orderly fashion

<img src=images/041-stop-wp.png />
***

Use the "Remove" button to remove the deployment and remove all containers

<img src=images/042-remove-wp.png />
***

Verify in the browser that the wordpress blog post is gone by refreshing the page

<img src=images/043-refresh.png />
***

Redeploy the Wordpress persistent stack.  Note, if you are on a multi-worker node instance, you will need to set the host constraint like this to ensure that the containers run on the same hosts as before, so they can re-join with the existing host volumes that were created on those hosts.

Within the Deploy ootions, set the host constraint for the Wordpress container

<img src=images/044-wp-constraint.png />
***

Then using the "+" icon in the UI, expose the database service snd set the host constraint for the Database container

<img src=images/045-db-constraint.png />
***

Select the "Deploy" button to deploy the stack

> *Important - Verify that the Wordpress and Database containers are running and deployed on the **same hosts as before**.  If this is not the case, Stop and Remove the deployment and set the Host constraints again.  The containers must run on the same hosts to reconnect with the existing host volumes.*

<img src=images/046-wp-redeploy.png />
***

Once the Deployment if running, healthy and green, navigate back to the blog post URL that you noted, in your browser and refresh the page

<img src=images/047-refresh-blog.png />
***

The data persisted because it was written to the host volume, and then re-joined to the containers when they were re-deployed on the same hosts.

> *Note - you can specify the exact hosts to run on when the Wordpress stack is deployed by using a host tag.  This will automatically set the host constraint to host tag and no adjustment will be needed at deployment time.  See*

**need to find resource for above**

## Running Applications Across Multiple Docker Hosts

Introduction to OCCS service discovery, running WordPress stack across multiple hosts

In this section we will explore how the Wordpress stack is able to deploy the Wordpress and Database container on different hosts, **and allow communication between the two.**

If you have a multi-host setup already with your Oracle Container Cloud Service, you may have experienced this already in the last exercise when you deployed the Wordpress persistent stack.  In this section we will explore how this works in more detail.

If you examine the Wordpress persistent YAML that we used by opening the stack advanced editor: 

Stacks page > Edit (Wordpress persistent) > Advanced Editor

<img src=images/048-wp-proxy.png />
***

Notice this template function in the Wordpress service section:

```
  	- "WORDPRESS_DB_HOST={{ proxy \"db:3306\" }}"
```

This is a  multi-host directive that routes the endpoint of given SERVICE_ID:PORT

The proxy directive creates a TCP proxy that the container uses to reach a remote service endpoint. When the container makes a connection to the endpoint, the proxy looks up the given service in the Service Discovery database. If the proxy finds matching services, it proxies the incoming connection to it. 

If you have not experienced deploying the stack across multiple hosts, and you do have a multi-host setup, feel free to re-deploy the stack, using the constraint function to force running the containers on different hosts.

The template arguments and service discovery provide a consistent usage paradigm for the successful deployment of multi-container stacks across multiple hosts.

More information on the template function can be found in the docs, here:

**enter docs URL for templates**

## CI/CD Integration with GitHub, DockerHub and OCCS

> *Note: This section is optional, time permitting.  You may complete it after the HOL.*

Container Cloud Service can be adapted into any Continuous Integration and Continuous Deployment (CI/CD) process, since you can connect multiple registries as needed.  

As you saw in the previous exercise where we created an image (the Docker whale) and then pushed that image to a Docker image repository, then deployed it within OCCS, we basically ran through a very simple CI/CD example.

**Requirements: user account for Github and DockerHub**

> If you do not have a Github account, get a free one here: [https://github.com/join](https://github.com/join) 

Now, let's explore another method using GitHub, Docker Hub and Container Cloud Service.

> *Note - In this exercise you will build a Dockerfile from Github on Docker Hub, deploy the latest version, then modify the Index.html in Github to trigger an automated Docker image build in DockerHub, and then verify the new build and image as a running container.*

To begin, complete steps 1 to 7 in this exercise:

[http://www.oracle.com/webfolder/technetwork/tutorials/obe/cloud/container_cloud/deploying_an_app_from_occs/occs-deploy-an-app-obe.html](http://www.oracle.com/webfolder/technetwork/tutorials/obe/cloud/container_cloud/deploying_an_app_from_occs/occs-deploy-an-app-obe.html) 

Once you have completed step 7 in the above, follow these steps.

In your Github account navigate to the URL where you have forked the above Helloworld.  Replace your Github username in the below URL

https://github.com/*username*/docker-images/blob/master/ContainerCloud/images/docker-hello-world/

On the Github page, click on the link for "Index.html"

<img src=images/049-hw-index.png />
***

This is the HTML for the home page of the HelloWorld Demo from above.  

You are going to modify this Index.html to create a new "Hello Earth" page, this will automatically trigger a new image build in Docker hub.  You will then run the resulting container to observe the changes.

Edit the page via the pencil icon and make these changes

<img src=images/050-edit-index.png />
***

On line 8, change the background color to:  

```
black
```

Add a new line 9 with the text: 

```
color: white;
```

On line 14, edit the H2 header to this, replacing YourCity with the city of your Oracle Code event: 

```
<h2>Hello Earth from Oracle Code YourCity!</h2>
```

Create a new line after </body> on line 15, and add this line for an image of the earth: 

```
<img src="[http://www.freeimageslive.com/galleries/space/earth/pics/a17_h_148_22725.gif](http://www.freeimageslive.com/galleries/space/earth/pics/a17_h_148_22725.gif)"
```

Check to see that it looks just like this (edits are highlighted in the red boxes):

<img src=images/051-index-edits.png />
***

Scroll Down and Commit your Changes.  Add a description and press the "Commit Changes" button

<img src=images/052-commit-index.png />
***

This will trigger a new automated build in Docker Hub, which will run the Dockerfile, which incorporates the new changes in index.html as part of the build process.  

It will take a few minutes for this to complete in Docker Hub.  When it does, Success will be noted in the Status column.

<img src=images/053-docker-build.png />
***

Back in Container Cloud service, click on the running container for the Hello-World-Demo

<img src=images/054-docker-hw.png />
***

Manually stop the container to trigger an automated restart of the Deployment

<img src=images/055-stop-hw.png />
***

Back on the Deployments page, this will cause the deployment to stop and restart.  The restart is automatic, so give it a few seconds to cycle.

<img src=images/056-stopping-hw.png />
***

Once the deployment is restarted, verify the Host that the container is running on. 

*Note: on multi-host OCCS instances, containers can restart anywhere in the resource pool, unless orchestrated to a specific host, by a specific method such as tag.*

<img src=images/057-running-hw.png />
***

Visit the host’s IP on port 8080 and observe your changes and a new Hello Earth!

<img src=images058-hello-earth.png />
***

**Congratulations!**  You have successfully completed this Hands On Lab.

## Summary/Recap Pointer to Further Resources

* [Oracle Github](https://github.com/oracle/docker-images)

* [Oracle Container Registry](https://container-registry.oracle.com)

* [Oracle Container Registry Docs](http://docs.oracle.com/en/cloud/iaas/container-cloud/index.html)

Oracle Blogs:

* [Containers, Docker and Microservices](https://community.oracle.com/community/cloud_computing/containers-docker-and-microservices)

* [Container Cloud Service Blog](https://community.oracle.com/community/cloud_computing/infrastructure-as-a-service-iaas/oracle-container-cloud-service)

**DONE**

