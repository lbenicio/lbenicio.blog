---

type: "posts"
title: Deploy an application using AWS EKS
icon: fa-comment-alt
tags: aws deployment cloud tutorial
categories: ["deployment"]

date: "2021-09-09"
type: posts
image: https://github.com/lbenicio/lbenicio.blog

image_alt: https://github.com/lbenicio/lbenicio.blog

---



Amazon Web Services (AWS) Elastic Container Service for Kubernetes (EKS) is a highly scalable and reliable platform for deploying and managing containerized applications in a Kubernetes environment. In this blog post, we'll explore the steps required to deploy an application on AWS EKS and discuss the benefits of using this platform for your deployments.

## Deploy an application using AWS EKS

AWS EKS (Elastic Container Service for Kubernetes) is a managed Kubernetes service that makes it easier for you to run and manage containerized applications on AWS. In this blog post, we will learn how to deploy an application on AWS EKS.

To deploy an application on AWS EKS, you will need to have an AWS account, a basic understanding of Kubernetes, and a Dockerized application.

1. Create an EKS Cluster: The first step in deploying an application on AWS EKS is to create an EKS cluster. You can do this through the AWS Management Console or using the AWS CLI.

2. Launch EC2 Instances: After creating the EKS cluster, you will need to launch EC2 instances to run your application. You can do this through the AWS Management Console or using the AWS CLI.

3. Install Kubernetes CLI: To deploy an application on AWS EKS, you will need to have the Kubernetes CLI (kubectl) installed on your local machine. You can download it from the official Kubernetes website.

4. Configure kubectl: After installing kubectl, you will need to configure it to connect to your EKS cluster. You can do this by following the instructions in the AWS Management Console or using the AWS CLI.

5. Deploy Dockerized Application: Once you have connected to your EKS cluster, you can deploy your Dockerized application by creating a Kubernetes deployment and a Kubernetes service. You can do this by using kubectl apply to apply a YAML file that defines the deployment and service.

6. Verify Deployment: After deploying your application, you can verify the deployment by using kubectl get pods to see the status of your application’s pods and by using kubectl describe pod to see more detailed information about the pods.

7. ccess Application: To access your application, you will need to create a Kubernetes service that exposes your application to the internet. You can do this by using kubectl apply to apply a YAML file that defines the service.

Conclusion: Deploying an application on AWS EKS is a straightforward process that makes it easier for you to run and manage containerized applications on AWS. With AWS EKS, you can enjoy the benefits of Kubernetes without having to worry about the underlying infrastructure. So, if you’re ready to start deploying your application on AWS EKS, get started today!


## Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)