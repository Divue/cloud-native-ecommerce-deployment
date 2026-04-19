multimicroserices e commerce   building devopsifing microservices and maintaining it 

to learn devops practically we need 
1. write the app 
2. look for opensource projects that have demo apps
 why companies prepare demo apps 
    -> to give demonstration of a new services 
    
  Open Telemetry demo app why?
     1. shld hv microservices 2.shld be realtime 3.eleborate documentation  4.stable app
     
     
zero privilage-> first granting admin previledge to iam user and then removin the admin previledge and give only the required previledge to the iam user


ec2 - devops-proj 
ubuntu latest -> t2.large -> key pair -> allow ssh , auto assign pubic ip is enabled -> lauch instance 
ssh -i pemfile-name ubuntu@ip-add -> chmod 400 pemfile

install docker , kubectl, terrafrom  from the official docs


RUN THE PROJECT LOCALLY 
  use docker compose as it can run multiple containers at once and link them with one other with ease 

1. clone ultimate-devops-project-demo from abhishek 
2. docker compose up //to pull and run the images  it wont completely run coz no dist left on ec2 isntances coz vol size is 8gb limited 
so resize the volume    chieck space df -h
 	go to instance storage-volume 
 	modify vol size to 30gb from 8 wait for 10 mins unitl its updates  volume state in use
 	lsblk shows a new volume created but its still not in use
 	resize the file system now
 	run sudo apt install cloud-guest-utils 
 	sudo growpart /dev/xvda 1  (the root partition)
 	lsblk  now its changes to 29gv
 	sudo resize2fs /dev/svda1  (df -h now the avail space is changed to 22g) 
now run again docker compose up -d   (d to not see logs )

expose the port 8080 from the security-group inbound rule
allow all trafic or my ip or 0.0.0.0  lets go with all 




there is more than 20 microservices in this project 
we wont be workin on all those but rather multiple teams will be workin on different services lke payment services will have a differnt team , another team for UI etc


DOCKER
1. Containerize the microservices
3 stage DOckerfile->image->containerize it

lets first containerize golang , java and python as its most popular
product-catalog-service which is written in go
frontend-service in java
recomendation service in python

dont conatinerize all the microservices but just hte popular ones 



lets satrt wit go PRODUCT-CATALOG

we hv a monorepo architecture as of now 
every lang has its own dependensies file and its developers job to maintain it


1. go to repo src/product-catalog/readme 
	go to terminal cd to src/product-catalog (if info not availabe ask developer)
	export produ... from readme 
	sudo apt install golang-go
	
	every lang has its own dependensies file and its developers job to maintain it -> after the build is succesful run ./product-catalog that got newly crated
	
remove the product-catalog binary
remove the dockerfile and rewrote it on ur own
docker build -t d/product-catalog:v1 .
docker run d/product-catalog:v1

create docker file 



now lets move on to ad service
remove dockerfile
read teh readme file for this service

prerequistie to implement docker lifecycle is how to build the app locally that way we can write the dockerfile with ease 

this java ad service is build using gradle 
check if java is installe or not

first run the service loclly and only hten build the dockerfile




now lets move on to recomendation service written in python
now readme file has no instruction unlike other ones
to u can either ask the developer for it or else 
try understanding the code by urself 
read requirement file  it contains all the dependencies 
then go to main py file
write dockerfile 



docker init - works only with docker desktop
push the images to registry 
 docker login docker.io or anyotehr registry 
docker push divue/product-catalog:v1

docker compose 
on ur dockerhub make an organization and within it ull make ur payment repo etc
docker tag org-name/image-name:version

writing docker compose file
   1. create yml file
   	a. services: microservices pull/run   one section for each
   	b. networks: netowrk for microservices
   	c. volumes: volumes for microservices




INfrastructure as CODE
on vs code
hashicorp terrafomr extension , yaml extension, official terrafomr docs 

aws login -> aws cli -> aws configure
secretry credential -> access key -> cli 
mkdir eks (terrafrom)

remote backend, state lockin using dynamoDB, 
now we r writng terrafrom backend

always go for modular approach 
create a folder modules -> eks & vpc 
invoke the resources form the modules 


eks foler -> backend folder 
things to remember 
	start wit provider -> resource blocks -> variables file -> output file 
	
we need provider, s3, dynamoDB

always go for modular approach 
create a folder modules -> eks & vpc 
inside vpc 
write main.tf and invoke the module from main.tf
why create modules??

fro writn vpc we need vpc resource, pblic & private network 
internet gatewary, nat, route table 
every subnet has a route table 

vpc 
main , output, variable

writng eks module using terraform 
   to create a eks cluster we need 2 iam roles where 1 iam is for cluster(controle plane) and other is for nodes (data plane) attached a individaul policy to both 
 steps:
   iam roles for cluster -> policy -> eks cluster  for master plane 
   iam roles for policy -> policy -> eks cluster for node group -> attach it to eks cluster
   
to make module work we need ot invoke the modules to it to work 
now on the main eks folder on the main.tf write the config and invoke the modules 

we are not goin to invoke anytin in backedn repo, it was just to create the s3 bucket and dynamoDB

module "module_name" {source = 'path' variables}  to invoke a module 
terraffom init -> plan -> apply -> destroy





K8s
  in general there are 2 accounts 
  user acc- for users 
  service acc - is for services, service shld hv a service account for permission
  
  in everyname space k8s always hv a default service account
   k8s assign defalt sa acc to pod 
  
  we dont assign a service accoutnt to pods on a demo project, it woks wit default sa,   every service requires a service account 
  
 Deployment resouce -> scaling n healing feature 
 Service resource -> service discorery solver, pod ip chnages everytime so frontend cnat communicate directly to backend as ip chnages
 service doesnt operate wit ip but rather labels and selectors
 it also allows external communication like user->pods
 
 we dont need to remember all the deployment ocdes but rather just hte skeleton of it 
 
 3 differnt service types- clusterIP/Nodeport/LB
 
 service uses selecter to match label wit pod or resource
   
   deploy project 
   kubectl config current-context
   kubectl get all 
   cd to k8s dir
   first create a service acc 
   kubectl apply -f serviceaccount.yaml
   kubectl get sa // verfy 
   kubectl apply -f complete-deploy.yaml //will create sercie n dep of all
   kubectl get pods //verify and wait till all pods r runing
   kubectl get svc // see all services 
   
   how one sercie is connected to others?
   if shippin has to connect to quote service then in hte env variable of shippin pod i have the service name of the quote service 
   
   how to access the project like forntend proxy ?
   kubectl get svc | grep frontendproxy // gives priveate ip 
   cannot access private ip
   whn we tried to acces the frontend proxy we cant 
   for that we need to know 3 types of service are there
   clusterip       nodeport   loadbalancer 
   k8s is within vpc, internal k8s has its own network , vpc netowrk is connect ot nodes of eks cluster, change service type to nodeport and htn we can access the frontend proxy service, if we cna access the ip of nodes ten we cna access the pods from within the organization or cluster
   to access it from the net then change service type to loadbalancer
   
   drawbacks of loadbalancer
   1. not declarative (explain)
   2. not cost effective 
   3. lack of flexibility
    GO for ingress controller 
    
   Ingreass and ingress controller
     helps in defining routing rules for incoming trafic in cluster
     
    follow the docs install alb ingress controller
    
    
    costum domain configuratoin
    1. get any domain
    2. connect the domain with loadbalancer dns or route 53 (hosted zone for this case)
    
    
    
    
    
CI/CD

Github acions
within the organization we'll only build ci/cd for the development team we are workin with thats it
 writing the ci for each microservices
  
  basic structure claude will explina
  
  
  go to repo settings -> actions secrets-> add new secret with DOCKER_Username and DOCKER_TOken and GITHUB_TOKEN
  executing workflow
  
Gitops using argoCD
  using offcila site
  kubectl creaet namespace argocd 
  
  
  