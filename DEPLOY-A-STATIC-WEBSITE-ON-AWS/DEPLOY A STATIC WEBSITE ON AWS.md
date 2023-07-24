# DEPLOY A STATIC WEBSITE ON AWS

## VPC SETUP

Create a VPC with options as shown in the image below -

<img width="342" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/b9d1f910-b358-4125-b730-4dc8a4bbbc88">

Enable **"DNS HOSTNAMES"** in the VPC settings. (**This is done so that instances launched in the VPC receive public DNS hostnames that correspond to their public IP addresses.**)

![Alt text](/images/image-1.png)

As highlighted above, click on the **Actions** drop down and the select "**Edit VPC settings**"

![Alt text](/images/image-2.png)

Ensure "**Enable DNS hostnames**" is checked, then save.

Create an Internet gateway and attach it to the VPC(**An internet gateway allows internet access into our VPC**)

![Alt text](image-3.png)

Create public and private subnets in availability zones

![Alt text](image.png)
