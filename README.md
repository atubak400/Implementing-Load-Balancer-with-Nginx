# Implementing-Load-Balancer-with-Nginx
Implementing Load Balancer with Nginx is a common task in many production environments. 

![Introduction](./img/1.png)

## A. Introduction to Load Balancing and Nginx
Load balancing is the practice of distributing incoming network traffic or workload across multiple servers, services, or resources to ensure optimal utilization of resources, improve response times, maximize throughput, and enhance the reliability and availability of applications and services.

Nginx (pronounced "engine-x") is known for its ability to efficiently handle a large number of concurrent connections and serve web content quickly.

## B. Setting Up a Basic Load Balancer
We are going to be provisioning two EC2 instances running ubuntu 22.04 and install apache webserver in them. We will open port 8000 to allow traffic from anywhere, and finaly update the default page of the webservers to display their public IP address.

Next we will provision another EC2 instance running buntu 22.04, this time we will install Nginx and configure it to act as a load balancer distributing traffic across the webservers.