## Containerization-and-Deployment-of-nodejs-Application - pt2

### Goal:

### To deploy an e-commerce application developed using React utilizing GitOps 

### The project workflow is shown below:


  ![Project1-diagram1](https://github.com/Noettie/End-to-End-automated-CI-CD-Pipeline-utilizing-GitOps-PART-ONE/assets/108426517/b56293f8-f11e-4745-80eb-edb06a1f4eb1) 


 :bookmark: 
 ### This part of the project is the Continous Deployment part. 

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
** Jenkins updates Manifest repo with new image
** Argocd deploys new application onto kubernetes cluster

NB: Two repositories required for 2 Jenkins jobs.



  
