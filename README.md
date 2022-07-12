
# Node.Js Documentation
## _Table of contents_
- Introduction to the Node.js
- Required Tools
- Tools and Definitions
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

## Tools and Definitions
This guide uses the following terms and definitions:
 | Tools | Definition |
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
### Stage-I 
#### Creation of VPC and EKS Cluster using Terraform and GitHub Configuration.
Initially,we created a EC2 Instance in AWS console and install Terraform software on it. Terraform is an open source infrastructure as code (IaC) software tool,using terraform code we installed VPC(Virtual Private Cloud) and Kubernetes Cluster with one nodegroup with minimum 1 spot machine and maximum 5.
 >For Installing Terraform software, we used this link 
 https://cloudlinuxtech.com/install-terraform-on-ubuntu-uninstall-terraform/
 
Then, we created a Organisation in GitHub and made two repositories named one as Production-team and another as Development-team. In the Production-team repository,we kept our Project Code.

After this,we made Git Branch Protection rules in GitHub.
* Branching is the cornerstone of cooperative work using Git. Developers utilize branches to work on the same source code repository in parallel. Generally speaking, when working with branches, there is one main branch in a repository from which various developers create their own additional, diverging branches. Once a developer’s project is done, they then merge their side branch back into the main branch.
* The ability to create and later merge branches introduces a risk, however: Developers may inadvertently push insecure content into the Git repository’s main branch.  In order to minimize risk, source code management platforms introduced branch protection rules.
> For Git Branch protection rules, we followed this link,
https://spectralops.io/blog/how-to-set-up-git-branch-protection-rules/

### Stage-II
#### Installation of Required softwares in Jumpbox and creating a Different Environments for Project.
In this stage, we installed Docker, Jenkins, helm, and  other required softwares in jumpbox and also we created a ECR(Elastic Container Registry) in AWS console.
Then we created a Different environments like Dev environment, qa environment and Prod environment.Dev environment for development team and qa environment for testing team and Prod environment for Production team.
> For Docker Installation, we reffered this link 
https://phoenixnap.com/kb/install-docker-on-ubuntu-20-04

After installing the Docker software, Using the Nodejs project code, we wrote a 
Dockerfile and test it by running the dockerfile and its successfully created a dockerimage and we pushed that image to the DockerHub.
> For Installation of Jenkins software, we used this link https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-20-04

After installing Jenkins, we created a  three pipeline script jenkins job for dev, qa and prod environment.

> For Installation of Helm , we reffered this link
https://phoenixnap.com/kb/install-helm

After installing helm, we created a repository in GitHub named as devops and kept the helm chart for nodejs project in this repository. In helm chart we did certain changes in values.yaml file and created separate values.yaml file for dev,qa and prod environment.

### Stage III 
#### Creating jenkins job for different environment and setting alerts for job state. 
##### Pipeline script for Dev Environment

In this Jenkins job,we kept Pipeline script in the  devops repo. It will run a script which will pull the nodejs repo and run the docker build and upload the image to ECR.we have made a configuration that, when there is merge happens to main branch , then this job will trigger automatically and we integrate slack to jenkins and set an alert to slack when the job fails/succeed.

##### Pipeline script for QA Environment

In this jenkins job, we used Same script to deploy to QA environment.we Pulled the latest image from the Dev environment and deploy it in the QA Environment and we configure a jenkins to slack and set an alert to slack when the job succeed/fails.

##### Pipeline script for Prod Environment
In this jenkins job, we used Same script to deploy to prod environment.we Pulled the latest image from the QA environment and deploy it in the Prod Environment  and we configure a jenkins to slack and set an alert to slack when the job succeed/fails.


### Stage IV
#### Deploying Metric server,Cluster Autoscalar and HPA(Horizontal Pod Autoscalar)

- [MetricServer][df1] - Metrics Server is a scalable, efficient source of container resource metrics for Kubernetes built-in autoscaling pipelines. The Metrics server role is it will frequently checking the metrics of every running pods in EKS cluster. The main role of this server is it will help to Horizontal Pod Autoscaler and Vertical Pod Autoscaler.

> Metrics Server can be installed either directly from YAML manifest or via the official Helm chart. To install the latest Metrics Server release from the components.yaml manifest, we run the following command to install metric server.
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

- [Cluster Autoscalar][df1] - cluster autoscaler automatically resizes the number of nodes in a given node pool, based on the demands of your workloads. You don't need to manually add or remove nodes or over-provision your node pools. Instead, you specify a minimum and maximum size for the node pool, and the rest is automatic. Cluster Autoscaler typically runs as a Deployment in your cluster.

> For Deploying cluster Autoscalar, we reffered this link
https://docs.aws.amazon.com/eks/latest/userguide/autoscaling.html

- [HPA(Horizontal Pod Autoscalar)][df1] - The Kubernetes Horizontal Pod Autoscaler automatically scales the number of pods in a deployment, replication controller, or replica set based on that resource's CPU utilization. This can help your applications scale out to meet increased demand or scale in when resources are not needed, thus freeing up your nodes for other applications. When you set a target CPU utilization percentage, the Horizontal Pod Autoscaler scales your application in or out to try to meet that target. To enable the HPA, we did changes in Prodvalues.yaml file and made HPA enabled in Prod  Environment. 

![alt test]([https://www.kubecost.com/images/hpa-overview.png](https://github.com/nveeresh133/doc-project/blob/main/HPA.drawio.png))
> For Installing HPA(Horizontal Pod Autoscalar), we used this link
https://docs.aws.amazon.com/eks/latest/userguide/horizontal-pod-autoscaler.html

### stage IV
#### Configuration of Monitoring Tools Prometheus and Grafana in EKS-Cluster

Prometheus, is a systems and service monitoring Tool. It collects metrics from configured targets at given intervals, evaluates rule expressions, displays the results, and can trigger alerts when specified conditions are observed. Grafana is an open source, feature rich metrics dashboard and it  allows to visualize the data stored in Prometheus (and other sources).and we created a dashboards in grafana to visualize the metrics and also imported some dashboards like Sample Dash Board IDS: 3119,7249 8919,6417.
> For Prometheus installation, we followed this website
https://linuxhint.com/install-prometheus-on-ubuntu/
For Installing Grafana ,we referred this link,
https://computingforgeeks.com/how-to-install-grafana-on-ubuntu-linux-2/

For Monitoring Kubernetes cluster and services, we configure Prometheus and grafana using a helm.The Prometheus Node Exporter exposes a wide variety of hardware- and kernel-related metrics.That will periodically (every 1 second) gather all the metrics of the system. It will monitor our filesystems, disks, CPUs, memory but also your network statistics.
Also we installed a Alert Manager and integrate this to Slack channel.Prometheus can generate alerts when a target is unavailable and send them to the Alert Manager, it sends an Slack notification to let you know that a target is down. Prometheus can send alerts to Alert Manager depending on any Prometheus metrics.
> For Installing and Configuring Alert Manager to Prometheus, we followed this website
https://linuxhint.com/install-configure-prometheus-alert-manager-ubuntu/

Then, we set some alerts for Kubernetes cluster like, when Pod is pending state,Pod is CrashLoopoff and Node is down and Hard disk space is low.when the target is unavilable then alert manager send an alert to the slack channel.

> For Setting Required alerts in Aler Manager, we referred this website
https://awesome-prometheus-alerts.grep.to/rules.html

![alt text](https://github.com/nveeresh133/doc-project/blob/main/promotheus%20diagram.drawio.png?raw=)

### Stage V
#### Setting Up of Logging Tools like EFK(Elasticsearch,Fluentd-bit,Kibana)
In this stage, we have done the EFK setup. EFK stands for Elasticsearch, Fluentd, and Kibana. is a popular and the best open-source choice for the Kubernetes log aggregation and analysis.
We Created a one more EC2 instance and installed Elasticsearch in it and Elasticsearch is a distributed and scalable search engine commonly used to sift through large volumes of log data.Its primary work is to store logs and retrive logs from fluent-bit. 
Then we installed Kibana software in this Ec2 machine and Kibana is UI tool for querying, data visualization and dashboards. It is a query engine which explores log data through a web interface, build visualizations for events log, query-specific to filter information for detecting issues.

After this, we deployed Fluent-bit in a kubernetes cluster as a DaemonSet because,Fluent-bit is a  log agent tool will need to run on every node to collect logs from every POD, hence it is deployed as a DaemonSet (a POD that runs on every node of the cluster).When Fluent Bit runs, it will read, parse and filter the logs of every POD and Later it transforms and ships to Elasticsearch backend. In Elasticsearch data are analyzed and later it moves to the Kibana and there the visualization of data takes place.

![alt test](https://github.com/nveeresh133/doc-project/blob/main/EFK.drawio.png)
> For Installing Elasticsearch and Kibana,we referred this link
https://phoenixnap.com/kb/how-to-install-elk-stack-on-ubuntu

> For Deploying the Fluent-bit in Cluster, we referred this website
https://docs.fluentbit.io/manual/v/1.3/installation/kubernetes

