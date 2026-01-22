**K8s resources Requests & limits**

H1 BI田

we can specify the resources to the container such as CPU and Memory.

when we specify the resources requests to the container inside pod, the kube-scheduler uses this information and decide onto which node the pod should place, when we specify resource limits to the container, kubelet ensures that the running container is not allowed to use more resources than the limits we set.

requests: requests means we specify value that the container should only use this much of CPU and memory.

Limits: Limits means if container need extra CPU and memory it should not crossed them than what we set in manifests.

for example a pod contains two containers which have requests like 0.25 CPU and 64MiB, and limits of 0.5 CPU and 128 MiB of memory. hat means we can say the pod has

requests of 0.5 CPU and 128 MiB of memory and limits of 1 CPU and 256MiB of memory.


**Example of resource for containers:**
resources:
request:

cpu: 250m

memory: 5Mi

limits:

cpu:1

memory: 5Mi

we create one directory, in that we add docker file (which have docker image), then app code files.

**Push Docker Image to ACR locally:**

I

2. Then we build docker image using docker build t image name, colon and tag of image then fullstop (fulstop means it add all files which are in that directory for

building the image) ex: docker build candom.imgtag.

3. To enable docker login in ACR, go to ACR->registeries->access keys->enable adminuser->we will get username and password, these credentials are used to push docker

image to ACR

4. To login to ACR: Execute docker login acr name or az acc login

5. To tag the image for push to ACR: docker tag <img name>: tag acrname/repository.name/imagename:tag, ex: docker tag candom image.tag

spandana.azurecr.io/repository_name/random-img.tag (we can give any random name for repository name)

6. To list out the images docker images

7. To push the image to ACR: docker push acrname/repository_name/image_name/image.tag


**To attach ACR to existing AKS**: az aks update -n <aks.cluster name> - <resource group name> -attach-acc <act name>

Build Pipeline task: Conyfiles task in yami pipeline is used to copy the files from one folder/source (DefaultworkingDirectory) to another folder/source (Artifactstaging Directory)

->To access the build artifacts in release pipeline, we need to add the "publish build artifacts" task in build yam) pipeline Push the Docker image to ACR using azure devops pipeline:

first we need to create the service connection between ACR and azure dexons, this is required when we are using starter pipeline instead of build and push the docker image to ACR yaml pipeline pre-built template

To create the service connection: go to project settings->pipeline->service connection->docker registry->select ACR->give azure account credentials-provide at n from drop down->give service connection name and save. (if we want to create the service connection b/w azure dexops and docker, at that time also we can select docker registry to make connection). Go to new

pipeline->select repo, where the docker file is present->starter pipeline->then in view assistant task, search for docker, under darker you can see bulld and push the docker Images to ACR, select that one. in this task we can give acc name, command as build and push, acr repository name, dockerfile path, tag of docker image, we can give tag to the docker image according the latest commit id of our repo like $(Build. SourceVersion). For tagging the image, we can add variable in yami pipeline ex: tag: '$(Build BuildID) means when we run the pipeline first time, buildid is 1, then for second run it

is 2, so the tag of image is in incremental mode like 1, 2, 3, 4, 5 etc.

**Dec 1st 2025**

first we neeed to create serviceconnection between azure devons and AKS, for easy to pull the AKS info, privileges and all. to do that choose service connection type

as k8s.

After Pushing we need to Deploy to AKS using deployment and service yaml maniests:

Whenever any developer make changes to the code file, using git command in vs code terminal we can see what changes he made, that is "more file naве", екс поре index.html, then we can see the code changes compare to previous version. Environments in ADO pipeline: In environment we can group the resources such as VM, KBS cluster, web app, these are Lacgetd by deployments from pipeline. In the same

environment we can create multiple clusters and VM's. for example, we have dev environment, in that we have multiple AKS cluster, so we can use environment section to group the multiple aks clusters or yn's to one environment.

for that, we need add a stage called deploy, below is the yaml pipeline code

one we done with the pushing of image to ACR, then add the "publish build artifacts task" next phase is deploy to AKS.
so for that , we need to add a stage called deploy, below is the yaml pipeline code.
To define the variable in yami pipeline:
variablename: 'value of variable

to refer them in tasks: $(variablename)

stage: Deploy

displayName: Deploy to AKS

steps:

task:deploy to Kubernetes

Inputs: aksname:

action:CreateSecret (this action is used for pulling the images from acr successfully, so this task or action provide access to kës nodes to pull the tauges, for authentication purpose we use it. we are creating the secret to allow imgae pull from ACR)

serviceconnectionname:

task: search for "deploy to AKS"

Inputs:

action: Deploy

we have these yami maniest in GitHub of kalyanreddy.

manifestfile path:

after load balancer created, using the LB FIP, we can access the application.

Azure DevOps Release Pipelines: To achieve the continuous delivery, we use release pipelines. we have pre-deployment and post-deployment approvers, to approve our continuous delivery pipeline, once they approve then only pipeline will run. when we publish the build artifact while build CI pipeline that is deploy to aks, manifest files are artifacts.

Let's deploy the application to dev, QA, stg, prod aks clusters:

Create dev, ga, stg, prod namespaces, using kubectl create ns dev, kubectl create ns ga and so on... to get them kubectl get ne then for each namespace create service connection in azure devons 4 service connections, because within AKS cluster we have 4 namespaces which will have separate aks objects. so, to deploy the application into different object we need diff service connection for isolated purpose, to mistakenly deploy into anothee abiects.

4 serv connection.

go to release pipeline->add dev stage
once dev stage is added, you can see option called "add", click on that to add another stage that is QA, if you want clone the dev stage, click on dev stage and then

go to release pipeline->add dev stage

after dev, ga, stg, prod, added. Add the tasks that is creteimagepull secret and deploy to k8s.

add pre-deployment approvers, select after stage option. For example let's assume QA stage need to deploy after dev stage is deployed, select QA stage and select trigger as "after stage", then from dropdown select "dev" as a stage.

click on "add" button and from dropdown select "clone stage" option.

Lab on Above Theory:

2. create azure devops org->create project->go to repo->copy 2nd option commands and paste them in notepad.

to VS code->create one folder->open it in terminal->initialize that directory as empty directory and add one docker and index.html file, now execute git add, git

create one directory, write dockerfile and one code file.

go commit, then paste the commands from notepad to vs code terminal and push them to azure repo.

3. write deployment yaml and service yaml manifest and push to repo

4. Create ACR, AKS and create service connection between azure and ADO

5. build and push the docker image to ACR using Build pipeline

6. Deploy to AKS

Docker file:

FROM nginx

WORKDIR /app

COPY index.html /app

Index.html file:

<DOCTYPE html>

<html>

<h1>Welcome to Azure Container Registry</h1>

<body>

<h1>welcome to acr</h1>

<h2>Build and Push Docker Image to ACR using Azure DevOps</h2>

<p>Application Version: v1</p>

</body>

</html>

Deployment yaml manifest:

apiversion:apps/v1

kind: Deployment

metadata:

name: Build and push docker image deployment

labels:

app:first-app

spec:

replicas: 1

selector:

matchLabels:

app:first-app

template:

metadata:

labels:

app:first-app

spec:

containers:

name: first-app-container image: html:1.12

imagePullPolicy:Always

ports: -containerPort: 80

Load Balancer Service yaml manifest:

apiVersion: v1

kind: Service

metadata:

name: first-app-service

labels:

label.)

spec:

app:first-app (we give labels name of deployment yami maniest in LB service yaml manifest, then only LB LB understands it should send the trafic to pods which have this

type: LoadBalancer
selector:

app:first-app

ports:

port: 80

targetPort: 80

Kubernetes.bxt

To isolate k8s objects we use namespaces, we can create namespace in imperative way and declarative also.

Imperative: Kubectl create ns <namespace.name>

Namespaces:

To get them: kubectl get ns

We should not delete the default namespace, because it has AKS cluster IP service.

To apply deployment maniest in namspace: kubectl apply f <maniest filename> -n <namespace name> while creating the namespace in declarative way, we define limit ranges such as CPU and Memory for namespace, those limits are used by container.

once we define limit ranges for namespace, we don't need to define resource section in deployment yaml file, because any container which is present within the namespaces, it uses that limits what we specified for the namespace. That means instead of specifying the resources like CPU and memory to each container in deployment yaml file, we can provide the default request and limits such as CPU and Memory to all containers within the namespace.

Namespace yaml manifest:

apiversion: v1

kind: namespace

metadata:

name: dev

limitranges yaml manifest:

apiversion: v1

metadata:

kind: LimitRanges

name: namespace limitranges
namespace:dev

spec: limits:

-default:

CRU: 500m

memory: 150Mi

LimitRequests:

cpu: 250m

In See measure in millicores, 1000 millicores equal to 1 virtual CPU(vCPU), memory measure in Bytes, Mi stands for Mebibytes. 1 Mi is equal to 2^18 bytes so, 500m means 0.5 CPU, memory 150*2^10 bytes.

memory: 100M1

once we define yaml manifest, we apply them and to get limits what we specified within the namespace: kubectl get limits -n dev

Resource Quota in Namespace: ResourceQuota is an object in k8s, which provides constraints to aggregate resource consumption per namespace, and also it can limit the quantity of k8s objects such as cou, memory, volumes, secrets, services within the namespace, which helps for stable performance of k8s cluster, because when we create the k8s cluster, we define preset configuration of it, that means only few nodes can present based on size, so in cesourcequota yaml file we can restrict the number of nodes, pods, cou and momory. so that cluster will be in stable performance. or example, we set the number of pods as 5, now we want to add one more pod, resource quota will not allows us to increase the pod.

ResouceQuota yaml manifest:

apiversion: v1

kind: ResourceQuota

name: ns-resource-quota

metadata:

namespace: dev

spec:

hard:

requests.cpu: "1"
requests.memory: 10M1

limits.memory: 50M1

pods: "5" configmaps: "5"

secrets: "5"

services: "5"

To get Resouce quota: kubectl get quotan dev (you get only quota name by this command) To get more details about quota: kubectl get quota <quota name> -n namespace name

Dec 6th 2025:

Ingress Context Path Based Routing: context path based routing means, based on context, or based on URL or based on path like app1/example.com and also we want to maintain multiple type of applications behind the nginx ingress controller, then we can go for ingress context path based routing.

traffic flow: cliet request->LB FIP->Ingress Controller->Ingress service->cluster IP->backend pods.

pre-requisites:

first we create static IP for azure standard Load Balancer, and then we attach that public ip to helm chart of nginx ingress controller at load balancer ip instruction.

then we create nginx ingress controller

ingress service with context path based rules, which have all application rules, and in that it will ask for backend service, in that backend service we give cluster IP address, so, for each application, we need to create cluster IP service. then we apply each application deployment yaml file and it's cluster ip service file, let's say we have 2 apps, then we need to create 2 deployment yaml files and 2 cluster ip service yaml files, and then ingress service yaml file.

Use Control Shift

Attach files by draggi

26°C

Mostly

Ingress service yani file, let's say 2 apps are behind ingress service (Ingress svc is used to define rules):
apixersion: we take ap
i version from k8s API

git.txt

kind: ingress

metadata:

name: ingress-contextpathbasedrouting

annotation:

kubernetes.io/ingress.classinginx (we are informing to ingress svc that, we are using nginx controller) spec:

rules:

-http:

paths:

-path: /app1

pathtype: prefix

backend:

service:

name: app1-nginx-cluster-ip-service (cluster ip svc name for application1)

port:

number: 80 (service port number)

-path: /app2

pathtype: prefix

backend:

service:

name: app2-nginx-cluster-ip-service (cluster ip svc name for application2) port:

number: 80

behind scenes: any request came from client, we are telling to ingress service that, if any request is coming from client, go to particular application using paths,

ports and backend service.

Note: In ingress service, we can define default backend application also, that means any user is browsing the ingress public in they will get only one application that is default backend application instead of app 1 and app2. and we define default backend application same as above the app1 and app2 rules.

What is Domain Register: Domain Register is a company which provides Internet domain Names. Example: GoDaddy, AWS Route 53. 1. Domain register company verifies the domain you want to use is available, and allow to purchase it. Once the domain is registered, you are the legal owner of that domain name.

**DNS Zones:**

domain name is unique in Domain Name System. DNS zone is used to host the DNS records for a particular domain. Azure DNS: Azure DNS is not a domain register, it is not capable of acting as domain register, but in app service, azure provides domain register.

1. Azure DNS allows us to host the DNS zone and manage the DNS records for a domain. so we purchase a domain called "kubeoncloud" in AWS route 53 and delegate this domain onto Azure DNS zone. Delegate means we change the AWS nameservers to Azure Nameservers, when we registered our domain in AWS, that domain is pointing to Aks

nameservers, so we are changing the nameservers to azure nameservers.

Delegate_Domain_name_from_AWS_Route53_to_Azure_DNS:

1. Create a DNS zone in azure by giving details like sub, RG and name for dos zone. 2. 2.

Once das zone created, you get 4 nameservers.

3. make a note of these 4 nmeservers and update them in Domain Register, here domain register is AWS Route 53 4.

To update them, go to your domain register, click on "edit nameservers" and update them to azure nameservers. 5. Then do pslookup for that domain name, it resolves to Azure DNS (it will take time to resolve to azure dns, half day or 24 hrs)

Dec 8th 2025: 9:30 PM

C

Networking in AKS: kubernetes provides two options "kubenet" and "Azure CNI"

Kubenet: 1.

by default, kubnet networking mode is created when we deploying the k8s cluster, the kubnet networking create one more VNET, that means cluster is in one net and ymss nodes, public ip of LB, LB, NSG, Route table these all are in one more VNET, that ynet is created by kubenet.

2. Nodes will get IP address from the subnet of yoet. 3. pods will get IP address from pods CIDR, you can see pods CIDR in aks cluster networking property.

4. sometimes system pods get the same ip address of the node. 5. we can create 110 pods per node with kubenet network plugin

How pods from different node can communicate with each other: by default pods from different node can communication with each other using the route table created by kubenet networking plug-in. where it create route table from one node to other node.

Dec 9th 2025: 12PM

Nodes will get IP address from the subnet of xnet.

Network Plug-in: Azure CNI: when we deployed AKS with network plugin as Azure Container Networking Interface, route table will not create. 1.

2. pods will also get IP address from the same subnet of ynet, here pods CIDR is empty. 3. we can create 30 pods per node with Azure CNI

1.

revie

4. In azure cni, the pods cide is empty and pods get in address from subnet

When an AKS cluster is deployed, Azure CNI pre-reserves a block of IP addresses from the Vliet subnet for each to wasted IPs if nodes are underutilized. configured to support (which defaults to 30 pods for Azure CNI and pod ip must be unique). This static allocation happens even if the pods aren't running yet, leading or each node, based on the maximum number of pods that node is

4. due to the above Issue, there is IP exechaution with azure CNI, that is why Azure CNI overlay comes into picture.

Difference b/w kubenet and Azure CNI:

nodes. 2. In kubenet: the pods get private in's from pod cide. in Azure CNI: in azure cnl, there is no pods cide it was empty, so the pods get in address from the sutinet like

1. the nodes get IP address from azure subnet in both kubenet and azure cni

Azure CNI overlay Mode: to avoid the in exebaution, we use azure cná overlay networking mode, because here we need to specify a Pod CIDR range for pods. This range should not overlap with node Miet's address space or other peered networks. then pods will get ip address from that pod side, not from the subnet of ynet. we use Azure CNI overlay mode when we want to scale large number of pods.

2. Nodes

Application Gateway Ingress Controller(AGIC): To expose protect our cluster using TLS policy and WAF firewall.

apps in aks cluster, we use agic. by using this we can avoid load balancer and pip for it. and also we can

Dec 10th 2025: 12:30PM:

What is Prometheus and Grafana: 1. Prometheus and Grafana are open source tools, they are used together in the field of monitoring and observability. where Prometheus collects metrics from monitoring services while Grafana provides a flexible platform for visualizing these metrics, logs in customizable dashboards.

2. Coming to Azure Managed Prometheus, it is enabled by azure monitor workspace (azure monitor workspace stores the Prometheus metrics and logs), which is used to collect and analyzing the data, this azure managed Prometheus is fully managed by Microsoft, meaning users do not have to operate the underlying infrastructure. 3. Azure Managed Grafana is a data visualization platform built on top of the Grafana software, it is fully managed by MS, it integrates with azure monitor managed service for promentheus allowing users to visualize the preomentheus metrics.

3. Enabling container logs, azure managed Prometheus, and Grafana in AKS cluster:

-> Go to AKS->monitoring->insights-> you can check checkboxes of container logs Prometheus and Grafana, go to advanced settings and create new log analytics for container logs, while creating container logs we can edit collection logs, like from which namespace we need logs, whether we need system logs, events, HPA logs everything we can edit, create new Prometheus, and create new Grafana by giving name to it.

->once we created, azure creates below resources behind:

1. Azure Monitor Workspace: It acts as data repository for storing Prometheus metrics from AKS cluster.

2. azure managed Prometheus: It is used to collects metrics from AKS cluster and automatically stores them in azure monitor workspace. 3. Azure Managed Grafana: Grafana connects to azure monitor workspace to query and visualize the metrics.

4. Azure Monitor Agent: AMA runs on AKS nodes, which collects only container logs, and sent to azure log analytics workspace using DCE based on DCR, these logs are

we can see DCE and DCR in Monitor service. DCR are automatically created because while we enabling the container logs in AKS, we edit whether we want system logs,

stored in table format with names such as Container Log or ContainerLogV2 5.

6. DCE is used to provide a consistent and secure method for the Azure Monitor Agent (AMA) to send data to specific destinations (like an Azure Monitor workspace or

HPA logs, based on them it creates data collection rule. then they sent to log analytics workspace.

Log Analytics workspace),

->go to AKS->Monitoring->logs->container logs->you can see run button->after it ran, we get logs. if we want we can write query to get container logs.

To see the container logs in AKS cluster:

>go to Prometheus->rule group->you can see rules created by DCR. we can click on Prometheus explorer to write the query to get the metrics.

To see Prometheus metrics: -

Grafana is integrated with Prometheus metrics to query the metrics and visualize the dashboard. on Grafana overview page->you can see Grafana endpoint (URL)->click on that, then we can see dashboard and metrics of our AKS cluster. in that dashboard we can see cpu and memory utilization and limits, kube system-with no of pods, cpu and memory request and limits within the namespace, we can see namespace details also.

To see Grafana: ->open Grafana->automatically

Dec 14th 12 PM:

1. Azure monitor agent runs on each node in AKS cluster, AMA collect the logs and sent them to Log analytics workspace, using DCE based on DCR. Generation: application running inside the container generates information logs and warning/error messages such as application started successfully on this port, user logged in with this ip, data connection fails, not able to connect with external api, latency issues.

Flow of Container Logs:

2. we can analyze these logs within portal using azure monitor or inside aks also we can view/analyze them.

Log Analytics: to store logs and query them.

Data Collection rules: data collection rule have, what type of data needs to collect from where and where to send them.

Flow of Metrics:

1. Metrics means how much resources being used by the container such as CPU/Memory, network in/out. 2. Metrics also collected by AMA from cluster components, and sent them azure monitor workspace which is integrated with DCE, workspace is like a central storage place

to store metrics in Prometheus, azure monitor workspace is backend solution for Prometheus.

3. when Grafana created, it is linked with azure monitor workspace, so we can visualize the metrics in dashboard by opening Grafana endpoint URL.

1. usually we see container logs in log analytics workspace or in inside AKS motoring section logs, but we can see container logs in Grafana also by selecting data

Create Grafana Dashboard to visualize the container logs from AKS cluster:

source as azure monitor. usually we integrate logs analytics workspace with the azure monitor, so we can select azure monitor as data source in Grafana. 2. Go to Grafana->click on endpoint URL->click on HOME->

click on connections->you can azure Monitor as Data source, if it is not there we can add it also-go back and click on DASHBOARD->click on NEW DASHBOARD->click on add visualization->select data source as azure monitor->then you will see the dashboard,->then select service as Logs->then select resource as AKS cluster, and resource group of pods, load balancer->apply->click on switch to tables to see logs in table format, then you can see all logs from azure monitor.->and save it.

3. we can write the queries and filter them based on namespaces/pods to get the logs.

ex: where pod podname'

it will give you 10 lines of logs related to that pod, after query written, click on outside of text, then you will get logs.

limit 10

4. we can filter the dashboard also by going to settings of dashboard->filters like we can filter our dashboard based on subscription, namespaces, by adding new

variable->in query type dropdown we can select sub, namespaces, RG and all.

Dec 17th 9:40 PM:

we expose our application using the HTTPS protocols, and We can encrypt the HTTPS traffic at Ingress and for pods also.

TLS certificate:

we apply TLS cert to ingress resource.

Securing the ingress resource using TLS cert: 1. Create AKS cluster and integrate with ACR, Create ingress controller using helm chart, Crate one public ip address and give random DNS name for it, and attach it to

I

ingress controller helm chart command at PIP. 2. Create two deployment yaml files with2 diff apps, and two cluster ip service.

3. we generate the self-signed certificated using open ssl (from that one public cert and one private key file generated)and store them in k8s secret, while creating the secret, we give public cert file name and private key file name, so that both stored in k8s secret. using this link(https://docs.azure.cn/en-us/aks/ingress-own-tls?tabs=azure-cli)
4. Create one ingress resource and define the path based rules in ingress resource and add tls cert configuration.

2. ingress resource pods and secret must be created in same namespace.

Dec 18th 4 PM:

Securing the secrets using CSI volume that uses azure key vault:

1. Create key vault and store one secret in it.

2. create user assigned managed identity

Dec 19th 9:30 PM:

when the user hits the app pods, the traffic flow is like user->LB fip->Node->Pod, by default, the externaltrafficpolicy is set to cluster, so SNAT is enabled, user address is translated into node private ip address and pods can see node internal ip address.

Preserving the Client IP address for pods: 1.

ip 2. while defining the k8s service let's say Load balancer svc, if we set the ExternalTrafficPolicy as Local, SNAT is disabled, then we can see the real client in address who are hitting the application pods, if we set it to cluster then we can see the Node private IP address.

3. we can view the client ip address in application logs, http headers.
