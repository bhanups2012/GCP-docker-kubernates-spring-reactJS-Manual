Will create a simple GCP project and downaload GCP sdk.
Will do local setup in SDK. Select a zone while local setup.

Install docker desktop: usefull link https://docs.docker.com/language/nodejs/build-images/
 will create a Dockerfile in spring project with below content. 

Java Spring setup start:------------------------------------
Dockerfile:
FROM openjdk:8-jdk-alpine
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]

Docker build:
docker build D://GCP//fbabe -t gcr.io/fba-be/fbabe:v1

Breakdown an docker  build cmd: 
D://GCP//fbabe -> an exact path where Dockerfile exist
-t -> -t is a tag name of docker img
gcr.io -> , gcr.io is a google registory container, 
fba-be -> GCP project name.
fbabe:v1 -> docker img name with version

Docker push: This will docker img to google registory container, 
We can have a look the exact img in GCP-> https://console.cloud.google.com/gcr/images/
docker push gcr.io/fba-be/fbabe:v1

Docker img run local cmd: 'gcr.io/fba-be/fbabe:v2' img name
docker run -p 8080:8080 gcr.io/fba-be/fbabe:v2
8080:8080 -> right one is the port where app will get hit.

usefull link: 
https://www.youtube.com/watch?v=ejwiMFJETdQ&t=2s
https://jhooq.com/deploy-spring-boot-microservices-on-kubernetes/#part-2
notes: java -jar app.jar
Java Spring setup end:------------------------------------


ReactJs setup start:------------------------------------
Dockerfile:
# Step 1
FROM node:10-alpine as build-step
RUN mkdir /app
WORKDIR /app
COPY package.json /app
RUN npm install
COPY . /app
RUN npm run build

# Stage 2
FROM nginx:1.17.1-alpine
COPY --from=build-step /app/build /usr/share/nginx/html

dockerignore:
/node_modules
/build
.git
*.md
.gitignore


Docker build:
docker build D://GCP//fbabe -t gcr.io/fba-be/fbafe:v1

Breakdown an docker  build cmd: 
D://GCP//fbabe -> an exact path where Dockerfile exist
-t -> -t is a tag name of docker img
gcr.io -> , gcr.io is a google registory container, 
fba-fe -> GCP project name.
fbabe:v1 -> docker img name with version

Docker push: This will docker img to google registory container, 
We can have a look the exact img in GCP-> https://console.cloud.google.com/gcr/images/
docker push gcr.io/fba-be/fbafe:v1

Docker img run local cmd: 'gcr.io/fba-be/fbabe:v1' img name
docker run -d -it  -p 3000:80/tcp gcr.io/fba-be/fbafe:v1
Note: 3000:80 here 80 is always default and 3000 we can give any port.

ReactJs setup end:------------------------------------

GCP SQL->MYSQL setup:
The setup is quite easy which we can do in this link ->https://console.cloud.google.com/sql/instances
We have do make public IP address to connect locallly the mysql instance, 
Default user cred is root/(password which you define setup time)
Note: we have to add our pc IP with SQL->CONNECTION->Authorised networks. We can find what is my IP address.


GCP Kubernates Engine set Up:
create a Kubernates cluster from here: https://console.cloud.google.com/kubernetes
 
once its created then we can connect and while connect we open it in cloud sell option from connect UI.

List all deployments: it will list all the deployments
	kubectl get deployments
	
List all service: it will list all the service
	kubectl get service
	
delete service or deployments:  it will delete service or deployement
Service: kubectl delete service <SERVICE-NAME>
Deployements: kubectl delete deployment <SERVICE-NAME> or kubectl delete deployments.app <SERVICE-NAME> 

Backend setup: Spring boot backend deploy from gcr.io docker image.

create deployment: it will create new deployment
kubectl create deployment fbabe --image=gcr.io/fba-be/fbabe:v1

Expose Deployment: it will expose the service on new IP with port 8080
 kubectl expose deployment fabbe --type=LoadBalancer --port=80 --target-port=8080


Frontend setup: ReactJs based frontend deploy from gcr.io docker image.

create deployment: it will create new deployment
kubectl create deployment fbafe --image=gcr.io/fba-be/fbafe:v1

Expose Deployment: it will expose the service on new IP with port 80
 kubectl expose deployment fabfe --type=LoadBalancer --port=80 --target-port=80
 
Will get an exposed EXTERNAL-IP. and PORTs.  We can hit that IP with port and use the service.
