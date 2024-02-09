
Once setup its exactly as same as installing via LXC's except right at the end you may want to setup rootless mode, in which a non-root user can run docker to avoid giving containers overly broad permissions from root.

You do this by adding the user in question to the `docker` group. 

To add the current user to the docker group you can run this command
```bash
sudo usermod -aG docker $USER 
```

If you would like to add another user to the `docker` group then swap out the $USER with the Linux username of them, this would be the login name and the name of the /home/ directory they own. 
