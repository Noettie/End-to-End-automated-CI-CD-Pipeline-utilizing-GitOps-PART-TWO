## Containerization-and-Deployment-of-nodejs-Application - pt2

### Goal:

### To deploy an e-commerce application developed using React utilizing GitOps 

### The project work-flow is shown below:


  ![Project1-diagram1](https://github.com/Noettie/End-to-End-automated-CI-CD-Pipeline-utilizing-GitOps-PART-ONE/assets/108426517/b56293f8-f11e-4745-80eb-edb06a1f4eb1) 


  
 ### :bookmark: This part of the project is called Continous Deployment. 

### Objectives

1. Deploy application on cluster using argocd.
2. Configure the domain name server
3. Deploy a TLS certificate to secure the application.
4. Configure webhooks in Jenkins.
5. Make changes to manifest files that trigger automated deployment onto argocd.

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

* _Install argocd_
* _Install helm_
*_ Register a domain name on namecheap/godaddy 

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

### Application creation:
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

### Ingress controller installation 

The below commands will install the ingress-controller resource:
15. kubectl create ns ingress-nginx
16.  helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
17.  helm repo update
18.  helm search repo ingress-nginx
19.  helm install my-nginx-con ingress-nginx/ingress-nginx -n ingress-nginx

18. Check the service ip
$ kubectl get svc -n ingress-nginx


19. Add your domain name on digital ocean
20. Create an A record in digital ocean to configure the domain to nginx controller.Enter a name and a subdomain will be populated, then select the load balancer which was created.
* _This connects the load balancer to the subdomain which will redirect traffic from the domain to the ingress controller_
* _The load balancer that sits behind the ingress and directs the traffic to the services_
21. Edit the ingress.yml file by commenting out the annotation and spec section
* _Since there is only one microservice for the application, only one path is needed_
22. Ddeploy the ingress
$ kubectl apply -f ingress.yml
23. Check the ingress resource.
$ kubectl get ingress
24. Access the application using the domain name your registered. 

This is a prototype amazon application used only for demo purposes.

![amazon-domain-live](https://github.com/Noettie/End-to-End-automated-CI-CD-Pipeline-utilizing-GitOps-PART-ONE/assets/108426517/d9b5d43c-a020-4072-8920-de6d17e5c53d)


Secure the application using TLS Certiface

25. Navigate to artifact hub and search for cert-manager.
Follow all instrucions to install cert manager CRDs.
https://artifacthub.io/packages/helm/kubeflow/certmanager

26. The install command will output

27. Deploy the issuer
$ kubectl apply -f issuer.yml
_Put your actual email on the issuer.yml._
$ kubectl get issuer
28. A secret is also created when the issuer is deployed.
$ kubectl get secret
29. Uncomment the spec and annotation section of the ingress.yml, and deploy the ingress.
$ kubectl apply -f ingress.yml

The application should be secure:

![secure-amazon-app](https://github.com/Noettie/End-to-End-automated-CI-CD-Pipeline-utilizing-GitOps-PART-ONE/assets/108426517/e69ef427-7525-45da-988c-dcea8620bb49)

Monitoring
Deploy Prometheous

30. Create a prometheous namespace
$ kubectl create ns prometheous
32. Navigate to artifact hub and search for Prometheous
Follow the instructions to install Prometheous on the cluster
33. Access the prometheous dashboard:
$ kubectl --namespace=prometheus port-forward deploy/my-prometheus-server 9090

The dashboard should be accessibel on localhost:9090

![prometheus-dashboard](https://github.com/Noettie/End-to-End-automated-CI-CD-Pipeline-utilizing-GitOps-PART-ONE/assets/108426517/b5ecf116-55e0-473e-8a63-0af44b5b710b)

Deploy Grafana

34. Create a grafana namespace
$ kubectl create ns grafana
35. Navigate to artifact hub and search for Grafana
Follow the instructions to install Grafana on the grafana namespcae
33. Copy and paste the export command outputted after installation
34.Access the grafana dashboard:
$ kubectl --namespace grafana port-forward $POD_NAME 3000

The dashboard should be accessible on localhost:3000

![grafana-loginpage](https://github.com/Noettie/End-to-End-automated-CI-CD-Pipeline-utilizing-GitOps-PART-ONE/assets/108426517/2143831d-45d3-4f7b-84c2-de5096622a50)

Configure webhooks on Jenkins

35. On the Jenkins dashboard, navigate to the upstream job ( build job) and select configure.
36. Select Github hook trigger for GITScm polling under 'Build Triggers'.
37. Navigate to settings on Githubs on the build repository and click webhooks. Add a webhook.
38. Add the payload url, ie , the jenkins url on the url section.
39. Apply and save the changes on jenkins.
40. Create a bug in the application. Make changes to the Cart microservice.
* Argocd is constantly watching the manifest repository looking for any change.
* When a change is made to the src code, this change will trigger a build on Jenkins and a new image of the microservice is created.
* The first build triggers the second job ( which contains manifest file) and automatically updates the manifest with the new image in the deployment.yml
* Argocd will pickup the changes on the deploment file and deploys the application into the kurbenetes cluster.
* Argocd compares the state of the kurbernetes cluster and if there is a drift, it will effect the new changes.
  
![amazon-cd-jenkins](https://github.com/Noettie/End-to-End-automated-CI-CD-Pipeline-utilizing-GitOps-PART-ONE/assets/108426517/219956c0-0803-4f2f-b362-c4b8a68fd766)

### Congratulations you have successfully completed a CICD pipeline for an e-commerce application !




  
