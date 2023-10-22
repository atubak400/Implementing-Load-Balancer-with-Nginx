# Implementing-Load-Balancer-with-Nginx
Implementing Load Balancer with Nginx is a common task in many production environments. 

![Introduction](./img/1.png)

## A. Introduction to Load Balancing and Nginx
Load balancing is the practice of distributing incoming network traffic or workload across multiple servers, services, or resources to ensure optimal utilization of resources, improve response times, maximize throughput, and enhance the reliability and availability of applications and services.

Nginx (pronounced "engine-x") is known for its ability to efficiently handle a large number of concurrent connections and serve web content quickly.

![Load Balancing and Nginx](./img/2a.png)

## B. Setting Up a Basic Load Balancer
We are going to be provisioning two EC2 instances running ubuntu 22.04 and install apache webserver in them. We will open port 8000 to allow traffic from anywhere, and finaly update the default page of the webservers to display their public IP address.

![Load Balancing and Nginx](./img/2aa.png)

Next we will provision another EC2 instance running buntu 22.04, this time we will install Nginx and configure it to act as a load balancer distributing traffic across the webservers.

### 1. Provision EC2 Instances
EC2 instances are created through the AWS Management Console or CLI, allowing you to select configurations and enhance availability by placing them in specific Availability Zones. 

> Open your AWS Management Console, click on EC2. Scroll down the page and click on Launch instance:

![Provision EC2 Instances](./img/2b.png)

> Under Applications and OS Images, click on quick start and click on ubuntu:

![Provision EC2 Instances](./img/3a.png)

> Instances launched

![Provision EC2 Instances](./img/3.png)


### 2. Open Port 8000 in the security group of each of our webservers

To secure web traffic on port 8000, modify the associated security group's rules within the AWS EC2 dashboard, ensuring that access is restricted to trusted sources.

> Select your instance, scroll down the security tab, and select instance security group

![Provision EC2 Instances](./img/4.png)

> Modify access to allow traffic on port 8000

![Provision EC2 Instances](./img/5.png)


### 3. Install Apache Webserver
Installing the Apache web server on an Ubuntu server is a fundamental step in setting up a web hosting environment. This process ensures that your server can serve web content, making it accessible to users over the internet.

![Provision EC2 Instances](./img/6.png)

> SSH into these instances using putty or another terminal client (or use AWS's session manager). 

![Provision EC2 Instances](./img/7.png)

> Update package list by running 

```
sudo apt update -y &&  sudo apt install apache2 -y
```

![Provision EC2 Instances](./img/8.png)

> Verify that apache is running using the command below

```
sudo systemctl status apache2
```

![Provision EC2 Instances](./img/9a.png)


### 4. Configure Apache to serve a page showing its public IP

Configure Apache to serve a page showing its public IP: Customize Apache's configuration files to create a page that displays the server's public IP address, simplifying the verification of the Apache web server's functionality.

We will start by configuring Apache webserver to serve content on port 8000 instead of its default which is port 80. Then we will create a new index.html file. The file will contain code to display the public IP of the EC2 instance. We will then override apache webserver's default html file with our new file.

> Using your text editor(eg vi, nano) open the file /etc/apache2/ports.conf

```
sudo nano /etc/apache2/ports.conf 

```

> Add a new Listen directive for port 8000:

![Provision EC2 Instances](./img/10.png)

> Open the file /etc/apache2/sites-available/000-default.conf and change port 80 on the virtualhost to 8000 like the screenshot below:

```
sudo nano /etc/apache2/sites-available/000-default.conf
```

![Provision EC2 Instances](./img/11.png)

> Save and exit from the text editor. Restart apache to load the new configuration using the command below:

```
sudo systemctl restart apache2
```

> Create a new html file:

```
sudo nano index.html
```

> Paste the html file below. Before pasting the html file, get the public IP of your EC2 instance from AWS Management Console and replace the placeholder text for IP address in the html file.

```
        <!DOCTYPE html>
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: YOUR_PUBLIC_IP</p>
        </body>
        </html>
```

![Provision EC2 Instances](./img/12.png)

> Change file ownership of the index.html file with the command below:

```
sudo chown www-data:www-data ./index.html
```

> Overriding the Default html file of Apache Webserver:

```
sudo cp -f ./index.html /var/www/html/index.html
```

> Restart the webserver to load the new configuration using the command below:

```
sudo systemctl restart apache2
```
![Provision EC2 Instances](./img/13.png)

> When you visit **http://"your-public-ip":8000** on your web browser you should find a page like so:

```
http://"your-public-ip":8000
```

![Provision EC2 Instances](./img/14.png)
![Provision EC2 Instances](./img/15.png)

### 5. Configuring Nginx as a Load Balancer




