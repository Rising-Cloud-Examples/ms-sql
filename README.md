# ms-sql
Microsoft SQL Server running on Rising Cloud. You can find all the instructions to build this example at: https://risingcloud.com/docs/ms-sql

This guide will walk you through the simple steps needed to build and run Microsoft SQL Server on Rising Cloud. You can clone all the files needed for this example from our GitHub repository at: https://github.com/Rising-Cloud-Examples/ms-sql

Note: For all stateful applications, data must be stored within the /app directory of the stateful container to be retained in the event of a worker restart.

1. Install the Rising Cloud Command Line Interface (CLI)
In order to run the Rising Cloud commands in this guide, you will need to install the Rising Cloud Command Line Interface. This program provides you with the utilities to setup your Rising Cloud Task or Rising Cloud Web Service, upload your application to Rising Cloud, setup authentication, and more. If you haven’t already, click here to for instructions to install Rising Cloud CLI.

2. Login to Rising Cloud Using the CLI
Using a command line console (called terminal on Mac OS X and command prompt on Windows) run the Rising Cloud login command. The interface will request your Rising Cloud email address and password.

risingcloud login
3. Initialize Your Rising Cloud Web Service
Create a new directory to place your project files in, then open this directory with your command line.

Using the command line in your project directory, run the following command replacing $TASK with your unique task name.

Your unique task name must be at least 12 characters long and consist of only alphanumeric characters and hyphens (-). This task name is unique to all tasks on Rising Cloud. A unique URL will be provided to you for sending jobs to your task.

If a task name is not available, the CLI will return with an error so you can try again.

risingcloud init -st $TASK_URL
This creates a risingcloud.yaml file in your project directory. This file can be used to configure the build script.

4. Configure your Rising Cloud Web Service
Configure your risingcloud.yaml

In the previous step, a risingcloud.yaml file should have generated in your project directory. Open that file and set the Port and Port Mapping and ENV fields.  For all HTTP traffic requiring a load balancer, use the Port field.   For all other traffic, use Port Mapping.  Note that 80 and 443 are reserved for HTTP traffic.  If your application does not communicate over HTTP, the port field is required but can be set to any field you wish. 

port: 443
portMapping: 
     1433: 1433
env:
  ACCEPT_EULA: "Y"
5. Create your Dockerfile
Within your project directory, create a new file Dockerfile to tell Rising Cloud how to build and run your application. Within the file paste the following.  This uses the latest Microsoft SQL Server Container from Docker Hub, please change the version to whatever version you wish.  

FROM mcr.microsoft.com/mssql/server:latest

EXPOSE 1433
6. Build and Deploy Your Rising Cloud Web Service
Use the push command to push your updated risingcloud.yaml for your Web Service on Rising Cloud.

risingcloud push
Use the build command to zip, upload, and build your app on Rising Cloud.

risingcloud build
Use the deploy command to deploy your app as soon as the build is complete. Change $TASK to your unique task name.

risingcloud deploy $TASK
Alternatively, you could also use a combination to push, build and deploy all at once.

risingcloud build -r -d
Rising Cloud will now build out the infrastructure necessary to run and scale your application including networking, load balancing and DNS. Allow DNS a few minutes to propagate and then your app will be ready and available to use!

7. Test Redis
After confirming your application is up and running, use your command line to access your application via SSH.

risingcloud ssh $TASK
Congratulations, you’ve successfully built Microsoft SQL Server on Rising Cloud!
