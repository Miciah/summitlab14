#**Lab 1: Overview of OpenShift Enterprise 2.0**

##**Assumptions**

This lab manual assumes that you are attending an instructor-led lab at Red Hat Summit and that you will be using this lab manual in conjunction with the lecture.  

This manual also assumes that you have been granted access to three Red Hat Enterprise Linux virtual servers with which to perform the exercises in it.  If you do not have access to your servers, please notify the instructor.

A working knowledge of SSH, Git, and Yum and familiarity with a Linux-based text editor are assumed.  If you do not have an understanding of any of these technologies, please let the instructor know.

##**What you can expect to learn from this lab**

At the conclusion of this lab, you should have a solid understanding of how to install and configure OpenShift Enterprise 2.0.  You should also feel comfortable creating and deploying applications using the OpenShift Enterprise management console, using the OpenShift Enterprise administration console, using command-line tools, and managing the application lifecycle.

##**Overview of OpenShift Enterprise PaaS**

Platform as a Service is changing the way developers approach developing software. A developer typically uses a local sandbox with his or her preferred application server and only deploys locally on that instance. Developers typically start JBoss locally using the *startup.sh* command and drop their *.war* or *.ear* files into the deployment directory, and they are done.  Developers have a hard time understanding why deploying to the production infrastructure is such a time-consuming process.

System administrators understand the complexity of not only deploying the code, but procuring, provisioning, and maintaining a production-level system. They need to stay up to date on the latest security patches and errata, ensure the firewall is properly configured, maintain a consistent and reliable backup and restore plan, monitor the application and servers for CPU load, disk IO, HTTP requests, etc.

OpenShift Enterprise provides developers and IT organizations an auto-scaling cloud application platform for quickly deploying new applications on secure and scalable resources with minimal configuration and management headaches. This means increased developer productivity and a faster pace with which IT can support innovation.

This manual will walk you through the process of installing and configuring an OpenShift Enterprise 2.0 environment as part of this training class that you are attending.

##**Overview of IaaS**

One great thing about OpenShift Enterprise is that we are infrastructure agnostic. You can run OpenShift on bare metal, on virtualized instances, or on public/private cloud instances. The only thing that is required is Red Hat Enterprise Linux running on x86_64 architecture. We require Red Hat Enterprise Linux in order to take advantage of SELinux and other enterprise features so that you can ensure your installation is stable and secure.

What does this mean? This means that in order to take advantage of OpenShift Enterprise, you can use any existing resources that you have in your hardware pool today. It doesn't matter if your infrastructure is based on EC2, VMware, RHEV, Rackspace, OpenStack, CloudStack, or even bare metal, as we run on top of any Red Hat Enterprise Linux operating system running on x86_64.

For this training class, we will be using OpenStack as our Infrastructure as a Service layer.

##**Deploying OpenShift Enterprise**

In this lab, we are going to take advantage of the *openshift.sh* installation script, which automates the deployment and initial configuration of the OpenShift Enterprise platform.  However, for a deeper understanding of the internals of the platform, it is suggested that you read through the official [Deployment Guide](https://access.redhat.com/site/documentation/en-US/OpenShift_Enterprise/2/html-single/Deployment_Guide/index.html) for OpenShift Enterprise.

##**Electronic version of this document**

This lab manual contains many configuration items that will need to be performed on your broker and node hosts.  Manually typing in all of these values would be a tedious and error-prone effort.  To alleviate the risk of errors, and to let you concentrate on learning the material instead of typing tedious configuration items, an electronic version of the document is provided on your virtual servers.
    
    
**Lab 1 Complete!**

<!--BREAK-->
