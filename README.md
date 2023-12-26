## Containerization-and-Deployment-of-nodejs-Application - pt2

### Goal:

### To deploy an e-commerce application developed using React utilizing GitOps 

### The project workflow is shown below:


  ![Project1-diagram1](https://github.com/Noettie/End-to-End-automated-CI-CD-Pipeline-utilizing-GitOps-PART-ONE/assets/108426517/b56293f8-f11e-4745-80eb-edb06a1f4eb1) 


  
 ### :bookmark: This part of the project is called Continous Deployment. 

### Objectives

1. Containerize the application.
2. Push the docker image onto AWS Elastic Container Registry.
3. Deploy application onto digital ocean kubernetes cluster.
4. Make changes to manifest files that trigger automated deployment onto argocd.

### Tools:

<div>
  <img src="https://github.com/devicons/devicon/blob/master/icons/docker/docker-original-wordmark.svg" width="60"/>&nbsp;
  <img src="https://github.com/devicons/devicon/blob/master/icons/kubernetes/kubernetes-plain.svg" width="60"/>&nbsp;
  <img src="https://github.com/devicons/devicon/blob/master/icons/amazonwebservices/amazonwebservices-original-wordmark.svg" width="80"/>&nbsp;
  <img src="https://github.com/devicons/devicon/blob/master/icons/react/react-original-wordmark.svg" width="80"/>&nbsp;
  <img src="https://github.com/devicons/devicon/blob/master/icons/argocd/argocd-original-wordmark.svg" width="60"/>&nbsp;
  <img src="https://github.com/devicons/devicon/blob/master/icons/jenkins/jenkins-original.svg" width="60"/>&nbsp;
  <img src="https://github.com/devicons/devicon/blob/master/icons/digitalocean/digitalocean-original-wordmark.svg" width="60"/>&nbsp;
  <img src="https://github.com/devicons/devicon/blob/master/icons/github/github-original.svg" width="60"/>&nbsp;
  <img src="https://github.com/devicons/devicon/blob/master/icons/prometheus/prometheus-original-wordmark.svg" width="60"/>&nbsp;
  <img src="https://github.com/devicons/devicon/blob/master/icons/grafana/grafana-original-wordmark.svg"  width="60"/>&nbsp;
<div>

### Work-flow:
* Jenkins updates Manifest repo with new image
* Argocd deploys new application onto kubernetes cluster

_NB: Two repositories required for 2 Jenkins jobs._
### Prereguisites:
_Install argocd_

### CD Steps:

1. Create kubernetes cluster on digital ocean.
$ doctl kubernetes cluster create <your_cluster_name>
_When cluster creates, check the ui and connect to the cluster on the kubernetes dashboard on, "Connect to your cluster".
2. Paste the connection command on the terminal.
3. Confirm nodes where created
4. Navigate to the rgocd 'getting started page' and follow the instruction of installing the argocd resources. 
https://argo-cd.readthedocs.io/en/stable/getting_started/
Check the resources:
$ kubectl get pods -n argocd
$ kubectl get svc -n argocd
The argocd server is has a ClusterIP. Inorder to access the argocd server externally, a Load balancer is required.
5. Follow the instructions to accesss the Argocd API on the getting started page.
6. When the load balancer has been provisioned, the service type of the argocd server wil change to Load balancer. Use that IP to access the argicd ui.
7. Follow the login instructions to access the argocd dashboard.

8. Navigate to NEW APP. Give the applicaion a name, lets call it amazon-app.Project name is default.
9. On Source, paste the https url for the github repository with the manifest files.
10. Sync policy: Automatic for auto syncing of changes in the manifests in the git repository.
11. Path: " ./ " if the files are in the root directly
12. Destination: Use default.
13. Namespace:default.

Dashboard:
![creating-app](https://github.com/Noettie/End-to-End-automated-CI-CD-Pipeline-utilizing-GitOps-PART-ONE/assets/108426517/92cebb14-8e26-4666-97dd-bfaa0dbb43b9)

14. Create application. When application is created successfully, the dashboard will look like this:

![healthy_app](https://github.com/Noettie/End-to-End-automated-CI-CD-Pipeline-utilizing-GitOps-PART-ONE/assets/108426517/cdfcf01d-80c2-4e3c-8108-90794701d96a)


   
 









  
