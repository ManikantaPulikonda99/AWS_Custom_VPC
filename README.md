# AWS_Custom_VPC
Create a working custom VPC on AWS.

How to create custom AWS VPC?
------------------------------------------------------
---
### AWS VPC Setup Steps

1. **Create a VPC**
   - **CIDR:** `10.0.0.0/16`
   - **Tenancy:** `Default`
   - **Name:** `my_vpc`

2. **Note on Tenancy**
   - If you set **Tenancy = Dedicated**, AWS provisions compute resources on **Dedicated Hosts**, which is **more expensive**.

3. **Enable DNS Hostnames**
   - Go to **VPC Dashboard → Your VPC → Actions → Edit VPC Settings**
   - Enable **DNS Hostnames**

4. **Create Subnets**
   - Follow your network design (e.g., 2 Public and 2 Private subnets across 2 AZs)

5. **Configure Public Subnets**
   - Select each **Public Subnet → Actions → Edit Subnet Settings**
   - Enable **Auto-assign Public IPv4 Address**

6. **Create Route Table for Private Subnets**
   - **Name:** `Private RT`
   - Edit **Subnet Associations** and associate both **Private Subnets**

7. **Create and Attach Internet Gateway (IGW)**
   - Create an **IGW** and attach it to `my_vpc`

8. **Create a NAT Gateway**
   - Place it in **one of the Public Subnets**
   - Allocate a **new Elastic IP** during creation

9. **Update Private Route Table**
   - Edit **Routes** in the Private Route Table:
     - **Destination:** `0.0.0.0/0`
     - **Target:** NAT Gateway

10. **Configure Security Groups**
    - Either:
      - Create **one SG for all subnets**, or
      - Create **separate SGs** for Public and Private Subnets

11. **Update Main Route Table (for Public Subnets)**
    - Add route for outbound Internet access:
      - **Destination:** `0.0.0.0/0`
      - **Target:** Internet Gateway
    - Note:
      - Traffic within `10.0.0.0/16` is routed **locally** by the VPC Router
      - Traffic **outside `10.0.0.0/16`** goes out via the **IGW**

-------------------------------------------------------------------------------------

