This project shows the deployment of a container resource into a kubernetes cluster.
To deploy the following resources, ensure you have the following already installed on your machine
- kubernetes cluster / minikube 
- Docker 
- Git

Clone the repo 
# git pull https://github.com/onaiv22/tec 

I have already built the docker image and pushed it to my dockerhub.This is public so you should be able to pull the image down. The docker steps for this are below.

To run the yaml files, follow this order
creating the interview namespace
# kubectl create -f interview-namespace.yaml

To check if namespace is created
#kubectl get namespce -n interview

create deployment 
#kubectl create -f interview-deployment.yaml

To check if deployment is created
#kubeclt get deployment -n interview

#kubectl describe deployment -n interview

create service
#kubectl create -f interview-service.yaml

To check if service is created
#kubectl get service -n interview

#kubectl describe service -n interview

Create ingress.
Note - the ingress controller have used is Traefik, however there are other types you can choose from.
#kubectl create -f interview-ingress.yaml

#kubectl describe ingress -n interview

To get the endpoint to test with
#kubectl get ing -n interview

To deploy the the network-policy resource
Note this is implemented for pod isolation so pods within the ingress namespace can communicate with pods in the interview namespace. this is good for pod isolation where there are multiple namespace.
#kubectl create -f ingress-ns

To deploy the network policy
#kubectl create -f ingress-namespsce-nw-policy.yaml

NOTE - this dns name will not work as its a dummy, except you have this properly integrated into your environment and all set up in your dns config/route 53.
but to test the deployment you can curl the endpoint of the cluster IP with the port within the cluster or host you are running this from. to get the clusterIP, run this kubectl get service -n interview or kubectl describe service -n interview and the port in this case 80.

This could have also been easily deployed using helm. Helm is a package manager for Kubernetes that allows developers and operators to more easily package, configure, and deploy applications and services onto Kubernetes clusters. With HELM we wouldnt need to run each of this command which is time consuming.


Docker Image
Note: you dont have to do this section, but if you want to build the image and push to your own image repo then follow this:


A simple docker file which deploys an hello world app.
The docker image is using the base nginx docker image with a simple hello world hmtl file.
Note: I have just grabbed the html file from the internet, twicked it a bit but it works fine for the purpose of this task.

To deploy this:

Prerequisite to deployment:
 - ensure you have docker installed on your local machine
 - ensure you have git installed locally.

Pull down the repo locally:
  git pull https://github.com/onaiv22/tec

To build the docker image:
# docker build -t tec-nginx .

To tag the image before pushing it to your dockerhub:
# docker tag tec-nginx:latest your_dockerhub_username/tex-nginx:latest

ensure you are logged in into docker on your shell (docker login)
To push image into docker hub:
# docker push your_dockerhub_username/tex-nginx:latest





If you want to do build and tag images with your own docker repo ensure you change the image under spec in the deployment.yaml file, if not you should be able to use mine as the image in my deployment.yaml file is public.

To test the docker image locally - to ensure the container will work as expected:
Run the exec-script on the command line:
  ./exec-script

Note the exec-script builds the docker image and tags it with tec-nginx, it then runs the 
tec-nginx as a conainer. This is run in a detached mode so you have the shell back with the container still running.

Run docker ps to see the running container, I have exposed port 80 on the container and mapped/forwarded it to port 80 on your localhost.

To access the app:

Browser option: localhost:80

If running on a virtualised instance i.e vagrant box - grab the ip address of your vagrant box and the port.
e.g 10.10.45.11:80

Shell option: you can also curl that endpoint without using the browser from within the shell
