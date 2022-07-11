# doc-project
# Node.js documentation
## _Table of contents_
- Introduction to the Node.js
- Required Tools
- Terms and Definitions
- Overview of Project

## Introduction to the Node.js
The Objective of this Project is automating the deployment of given Nodejs web application in different environments like Dev,Qa and Production based on the requiements and using AWS as a cloud Platform and other tools for automation purpose. 
The Node.js is an open source project maintained on GitHub. To access the sources, report or
review issues, or contribute to the project, go to https://github.com/nveeresh133/AatmaaniProject.git

## Required Tools
For Automating and Deploying the Nodejs application we have used some tools like
- AWS
- Terraform
- Jenkins
- Docker
- Kubernetes
- Helm
- Ansible
- Prometheus
- Grafana
- EFK

## Terms and Definitions
This guide uses the following terms and definitions:
 | Term | Definition |
| ------ | ------ |
| AWS | AWS Amazon Web Services (AWS) is a secure cloud services platform, offering compute power, database storage, content delivery and other functionality to help businesses scale and grow. |
|Terraform |Terraform is a tool for building infrastructure with various technologies including AWS, Azure or GCP. Terraform enables you to create and manage infrastructure with code and codes can be stored in version control. |
| Jenkins |Jenkins is an open source continuous integration/continuous delivery and deployment (CI/CD) automation software DevOps tool written in the Java programming language. |
|Docker | Docker is open-source containerization software, docker used to build, ship, run applications as a containers. |
| Kubernetes | Kubernetes is Open-Source Container Orchestration software and it is used to managing the Containerized applications, container deployment and scaling and descaling the containers.|
| Helm | Helm is a Package Manager for Kubernetes applications. Helm Simplifies the proccess of Creating managing and deploying applications using helm charts.|
| Ansible | Ansible is an open sourceIT automation tool that automates provisioning, configuration management, application deployment, orchestration, and many other manual IT processes|
| Prometheus | Prometheus is a open-source software application used for event monitoring and alerting. It records real-time metrics in a time series database. |
| Grafana | Grafana is a multi-platform open source analytics and interactive visualization web application. It provides charts, graphs, and alerts for the web when connected to supported data sources.|
| EFK | EFK stands for Elasticsearch, Fluentd, and Kibana. EFK is a popular and the best open-source choice for the Kubernetes log aggregation and analysis.|
| npm | The Node.js package Uses npm to download and install the dependencies.|
| Git | A source control management system. You will need a git client if you want to checkout and use the Node.js sources.|

## Overview of the Project
### stage-I Creation of VPC and EKS Cluster using Terraform.
Initially,we created a EC2 Instance in AWS console and install Terraform software on it. Terraform is an open source infrastructure as code (IaC) software tool,using terraform code we installed VPC(Virtual Private Cloud) and Kubernetes Cluster with one nodegroup with minimum 1 spot machine and maximum 5.
![alt test](https://miro.medium.com/max/1400/1*9cdatdOvKgu4S_R89qzifA.png)

![alt test](https://miro.medium.com/max/1400/1*9cdatdOvKgu4S_R89qzifA.png) 
Then, we created a Organisation in GitHub and made two repositories named one as Production-team and another as Development-team. In the Production-team repository,we kept our Project Code.

After this,we made Git Branch Protection rules in GitHub.
* Branching is the cornerstone of cooperative work using Git. Developers utilize branches to work on the same source code repository in parallel. Generally speaking, when working with branches, there is one main branch in a repository from which various developers create their own additional, diverging branches. Once a developer’s project is done, they then merge their side branch back into the main branch.
* The ability to create and later merge branches introduces a risk, however: Developers may inadvertently push insecure content into the Git repository’s main branch.  In order to minimize risk, source code management platforms introduced branch protection rules.
> For Git Branch protection rules, we followed this link, https://spectralops.io/blog/how-to-set-up-git-branch-protection-rules/

### stage-II Installation of Required softwares in Jumpbox and creating a Different Environments for Project.
In this stage, we installed Docker, Jenkins, helm, and  other required softwares in jumpbox and also we created a ECR(Elastic Container Registry) in AWS console.
Then we created a Different environments like Dev environment, qa environment and Prod environment.Dev environment for development team and qa environment for testing team and Prod environment for Production team.
> For Docker Installation, we reffered this link https://phoenixnap.com/kb/install-docker-on-ubuntu-20-04

After installing the Docker software, Using the Nodejs project code, we wrote a 
Dockerfile and test it by running the dockerfile and its successfully created a dockerimage and we pushed that image to the DockerHub.
> For Installation of Jenkins software, we used this link https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-20-04

After installing Jenkins, we created a  three pipeline script jenkins job for dev, qa and prod environment.

> For Installation of Helm , we reffered this link https://phoenixnap.com/kb/install-helm

After installing helm, we created a repository in GitHub named as devops and kept the helm chart for nodejs project in this repository. In helm chart we did certain changes in values.yaml file and created separate values.yaml file for dev,qa and prod environment.

### stage III Creating jenkins job for different environment and setting alerts for job state. 
##### Pipeline script for Dev Environment

In this Jenkins job,we kept Pipeline script in the  devops repo. It will run a script which will pull the nodejs repo and run the docker build and upload the image to ECR.we have made a configuration that, when there is merge happens to main branch , then this job will trigger automatically and we integrate slack to jenkins and set an alert to slack when the job fails/succeed.

##### Pipeline script for QA Environment

In this jenkins job, we used Same script to deploy to QA environment.we Pulled the latest image from the Dev environment and deploy it in the QA Environment and we configure a jenkins to slack and set an alert to slack when the job succeed/fails.

##### Pipeline script for Prod Environment
In this jenkins job, we used Same script to deploy to prod environment.we Pulled the latest image from the QA environment and deploy it in the Prod Environment  and we configure a jenkins to slack and set an alert to slack when the job succeed/fails.


### Stage IV Deploying Metric server,Cluster Autoscalar and HPA(Horizontal Pod Autoscalar)

- [MetricServer][df1] - Metrics Server is a scalable, efficient source of container resource metrics for Kubernetes built-in autoscaling pipelines. The Metrics server role is it will frequently checking the metrics of every running pods in EKS cluster. The main role of this server is it will help to Horizontal Pod Autoscaler and Vertical Pod Autoscaler.

> Metrics Server can be installed either directly from YAML manifest or via the official Helm chart. To install the latest Metrics Server release from the components.yaml manifest, we run the following command.
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
