
Docker is an open-source project for automating and deploying applications. At some point you are going to run into an application that can either be installed via Docker or is built as a docker first application.

Docker is a runtime environment that needs to be installed on any of the machines that its deployed on. Its similar be required to install Java Runtime environment before being able to run java applications. 


Depending on the circumstance you may want to install Docker is slightly alternative ways, I have a list of three ways to install that I will rank from my preferred to last resort approach. 

* [Installing via Proxmox Scripts)

## Validate install

To Validate the installation was successful you can run this command, It will pull, run and display the outcome of the container to the user. 
```bash
docker run hello-world
```