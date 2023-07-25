# DEPLOY A STATIC WEBSITE ON AWS

<img width="930" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/38cab804-0133-4959-b2ea-72a14b766a19">

## VPC SETUP

Create a VPC with options as shown in the image below -

<img width="342" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/b9d1f910-b358-4125-b730-4dc8a4bbbc88">

Enable **"DNS HOSTNAMES"** in the VPC settings. (**This is done so that instances launched in the VPC receive public DNS hostnames that correspond to their public IP addresses.**)

<img width="839" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/af16da41-acc6-4e61-bba8-2227235e725b">

As highlighted above, click on the **Actions** drop down and the select "**Edit VPC settings**"

<img width="337" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/d001b659-1bef-4653-98d9-e07513f82f16">

Ensure "**Enable DNS hostnames**" is checked, then save.

Create an Internet gateway and attach it to the VPC(**An internet gateway allows internet access into our VPC**)

<img width="448" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/5b3e8f53-2b20-4852-8674-726dec80c437">

Create public and private subnets in availability zones

<img width="847" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/32dc36c8-1601-4689-8a22-6a076743c567">

Also enable "auto-assign-ip" so that IP's are assigned automatically to EC2 instances added into your VPC

<img width="328" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/780bab54-70a8-4c0f-81a2-5a5e019c6119">

Create a route-table (**This contains a set of rules, called routes, that are used to determine where network traffic from your subnet or gateway is directed. It simply tells network packets which way they need to go to get to their destination.**) - 

<img width="354" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/062b09f8-5ded-49eb-baa0-9c39740e7a94">

Add a route that connects to the Internet Gateway we created previously and associate the route table with public subnets. This means that all the traffic coming into the public subnets can only be from the **IG** and no other place.

<img width="463" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/96a08e66-d80e-4d51-894a-1b6be5cdc55c">

<img width="349" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/0fa77044-855b-41fe-b387-551887007450">

Create a NAT Gateway and assign it an IP address(**We use a NAT gateway so that instances in a private subnet can connect to services outside our VPC but external services cannot initiate a connection with those instances**) -

<img width="333" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/8efeb083-26d2-492f-96dd-474149b2c37f">

We would now create a new route table for the private subnets in both availability zones, then we would add routes, assign the NGW to the route table, and then assign the private subnets to them.

## SECURITY GROUP SETUP

<img width="859" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/080bc042-9728-4795-bf46-4e5fc5da2b49">

We will be creating 3 security groups, each with it's own function.
1. Application load balancer - We will be creating an inbound rule for this SG to allow HTTP & HTTPS traffic into our Webservers.
2. SSH - This will allow us access our webservers via ssh
3. Webserver security group

<img width="359" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/e880f5bd-2cdb-412a-b55c-30f8af018b42">

## SETUP APPLICATION LOAD BALANCER

### CREATE EC2 INSTANCES

Before we create the ALB, we would first spin up EC2 instances which would route their traffic through the ALB. Both EC2 instances would make use of the webserver SG created previously, we will also assign the "private app subnets" to both instances. Then we will add a bash script in the "user data" section to bootstrap the instance.

Link to the bash script - https://github.com/dipoesan/CLOUD-PROJECTS/blob/main/DEPLOY-A-STATIC-WEBSITE-ON-AWS/create_alb.txt

```bash
#!/bin/bash
sudo su
yum update -y
yum install -y httpd
cd /var/www/html
wget https://github.com/azeezsalu/jupiter/archive/refs/heads/main.zip
unzip main.zip
cp -r jupiter-main/* /var/www/html/
rm -rf jupiter-main main.zip
systemctl enable httpd 
systemctl start httpd
```

<img width="380" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/e2f636a5-b44e-49f3-bc1d-431f2b3ffac6">

### CREATE ALB

The ALB should be "internet-facing" and IPv4, select the preferred VPC and then select both availability zones in mappings. Assign the ALB SG, and then create a Target group and assign it to the listener you want to create.
