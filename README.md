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
