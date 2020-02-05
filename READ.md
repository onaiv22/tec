A simple docker file which deploys an hello world app.
The docker image is using the base nginx docker image with a simple hello world hmtl file.
Note: I have just grabbed the html file from the internet, twicked it a bit but it works fine for the purpose of this task.

To deploy this:

Prerequisite to deployment:
 - ensure you have docker installed on your local machine
 - ensure you have git installed locally.

Pull down the repo locally:
  git pull https://github.com/onaiv22/tec

Run the exec-script on the command line:
  ./exec-script

Note the exec-script builds the docker image and tags it with tec-nginx, it then runs the 
tec-nginx as a conainer. This is run in a detached mode so you have the shell back with the container still running.

Run docker ps to see the running container, I have exposed port 80 on the container and mapped/forwarded it to port 80 on your localhost.

To access the app:

Browser option: localhost:80

If running on a virtualised instance i.e vagrant box - grab the ip address of your vagrant box and the port.
e.g 10.10.45.11:80

Shell option: you can also curl that endpoint without using the browser from within the shell, run this on the command line
   curl localost:80
