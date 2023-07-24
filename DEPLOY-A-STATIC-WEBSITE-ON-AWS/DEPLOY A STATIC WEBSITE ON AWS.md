# DEPLOY A STATIC WEBSITE ON AWS

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

Add a route that connects to the Internet Gateway we created previously. This means that all the traffic coming into the public subnets can only be from the **IG** and no other place.

<img width="463" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/96a08e66-d80e-4d51-894a-1b6be5cdc55c">

<img width="348" alt="image" src="https://github.com/dipoesan/CLOUD-PROJECTS/assets/22638955/17120aeb-81fe-4380-ad45-cf8c40327325">

