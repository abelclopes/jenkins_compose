# jenkins_compose
Running Jenkins With Docker Compose
You can run a Jenkins controller with a single Docker command:

$ docker run -it -p 8080:8080 jenkins/jenkins:lts


```
version: '3.8'
services:
  jenkins:
    image: jenkins/jenkins:lts
    privileged: true
    user: root
    ports:
      - 8083:8080
      - 50003:50000
    container_name: my-jenkins-3
    volumes:
      - /home/adm-remote/jenkins_compose/jenkins_configuration:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
```
Line #1 is a comment.

Line #2 tells Docker Compose which version of the Compose specification we’re using. As of the time of this writing, 3.8 is the latest.

Line #3 starts defining services. For now, we just have one. 

The rest of the file defines the Jenkins container. 

It will run the latest Jenkins image with root privileges. We’re running the container with host networking, so lines #9 and #10 tell Docker to redirect ports 8080 and 50000 to the host’s network. 

The container’s name is jenkins.

Finally, /home/${myname}/jenkins_compose/jenkins_configuration is mapped to /var/jenkins_home in the container. Change /home/${myname} to your user’s home directory or the path you created the new directory in.




Run Jenkins Controller
Now it’s time to run your Jenkins controller. 

Run docker-compose in the directory where you placed docker-compose.yaml.

$ docker-compose up -d

Creating jenkins … done

Now point a web browser at port 8080 on your host system. You’ll see the unlock page.

Go back to your shell and view the logs with docker logs.

$ docker logs jenkins | less

Look for a block enclosed with six lines of asterisks like this:

```
*************************************************************
*************************************************************
*************************************************************
 
Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation:
 
c061b679107a4893b5383617729b5c6a
 
This may also be found at: /var/jenkins_home/secrets/initialAdminPassword
 
*************************************************************
*************************************************************
*************************************************************
```


