https://www.linkedin.com/posts/aditya-chikte-2176a21b5_aws-cloudcomputing-vpc-activity-7206652850477690880-A8OI?utm_source=share&utm_medium=member_desktop
# -Manually-Created-a-VPC


# Step-by-Step Guide to Manually Creating a VPC in AWS

This repository provides a step-by-step guide to manually creating a Virtual Private Cloud (VPC) in AWS.

## Prerequisites
- AWS account with access to the Free Tier
- Basic understanding of networking concepts like subnets, route tables, and internet gateways

## Step-by-Step Instructions

### Step 1: Sign In to AWS Management Console

1. Open the [AWS Management Console](https://aws.amazon.com/console/).
2. Sign in with your AWS account credentials.

### Step 2: Navigate to VPC Dashboard

1. In the AWS Management Console, navigate to the **VPC Dashboard**.
    - You can find VPC under **Networking & Content Delivery**, or search for **VPC** in the search bar.

### Step 3: Create a New VPC

1. In the VPC Dashboard, click **Your VPCs** in the left-hand menu.
2. Click the **Create VPC** button.
3. Enter the following details:
   - **Name tag:** Enter a name for your VPC (e.g., `MyCustomVPC`).
   - **IPv4 CIDR block:** Enter the IP address range for the VPC (e.g., `10.0.0.0/16`).
   - **IPv6 CIDR block:** (Optional) Select **No IPv6 CIDR Block** if you don't need it.
   - **Tenancy:** Select **Default** (this allows the use of shared hardware).
4. Click **Create VPC**.

### Step 4: Create Subnets

1. In the VPC Dashboard, click **Subnets** in the left-hand menu.
2. Click **Create subnet**.
3. Enter the following details for your first subnet (public subnet):
   - **Name tag:** Enter a name for the subnet (e.g., `PublicSubnet`).
   - **VPC:** Select the VPC you just created (`MyCustomVPC`).
   - **Availability Zone:** Choose an Availability Zone (e.g., `us-east-1a`).
   - **IPv4 CIDR block:** Enter a range for your public subnet (e.g., `10.0.1.0/24`).
4. Click **Add new subnet** to create a second subnet (private subnet):
   - **Name tag:** Enter a name for the subnet (e.g., `PrivateSubnet`).
   - **VPC:** Select the same VPC.
   - **Availability Zone:** Choose a different Availability Zone (e.g., `us-east-1b`).
   - **IPv4 CIDR block:** Enter a range for your private subnet (e.g., `10.0.2.0/24`).
5. Click **Create subnet**.

### Step 5: Create and Attach an Internet Gateway

1. In the VPC Dashboard, click **Internet Gateways** in the left-hand menu.
2. Click **Create Internet Gateway**.
3. Enter a name for your Internet Gateway (e.g., `MyInternetGateway`), and click **Create Internet Gateway**.
4. Once created, click **Actions**, and then select **Attach to VPC**.
5. Select your VPC (`MyCustomVPC`) and click **Attach Internet Gateway**.

### Step 6: Create Route Tables

1. In the VPC Dashboard, click **Route Tables** in the left-hand menu.
2. Click **Create route table**.
3. Enter the following details:
   - **Name tag:** Enter a name (e.g., `PublicRouteTable`).
   - **VPC:** Select your VPC (`MyCustomVPC`).
4. Click **Create route table**.
5. Select the new route table (`PublicRouteTable`), and click the **Routes** tab.
6. Click **Edit routes**, then **Add route**:
   - **Destination:** Enter `0.0.0.0/0` (this allows all traffic).
   - **Target:** Select your **Internet Gateway**.
7. Click **Save routes**.
8. Associate this route table with your **Public Subnet**:
   - Go to the **Subnet associations** tab.
   - Click **Edit subnet associations**.
   - Select your **Public Subnet** and click **Save**.

### Step 7: Create a NAT Gateway for Private Subnet (Optional)

1. In the VPC Dashboard, click **NAT Gateways** in the left-hand menu.
2. Click **Create NAT Gateway**.
3. Enter the following details:
   - **Subnet:** Select your **Public Subnet**.
   - **Elastic IP allocation ID:** Click **Allocate Elastic IP**, then **Allocate**.
4. Click **Create NAT Gateway**.
5. Wait until the status changes to **Available**.

### Step 8: Configure Route Table for Private Subnet

1. In the **Route Tables** section, create another route table for the private subnet:
   - **Name tag:** Enter a name (e.g., `PrivateRouteTable`).
   - **VPC:** Select your VPC (`MyCustomVPC`).
2. Click **Create route table**.
3. Select the newly created route table (`PrivateRouteTable`), and go to the **Routes** tab.
4. Click **Edit routes**, then **Add route**:
   - **Destination:** Enter `0.0.0.0/0`.
   - **Target:** Select the **NAT Gateway** you created.
5. Click **Save routes**.
6. Associate this route table with your **Private Subnet**:
   - Go to the **Subnet associations** tab.
   - Click **Edit subnet associations**.
   - Select your **Private Subnet** and click **Save**.

### Step 9: Security Groups

1. In the VPC Dashboard, click **Security Groups** in the left-hand menu.
2. Click **Create security group**.
3. Enter the following details:
   - **Name tag:** Enter a name for the security group (e.g., `PublicSecurityGroup`).
   - **VPC:** Select your VPC (`MyCustomVPC`).
   - **Description:** Describe the security group (e.g., `Allow SSH and HTTP traffic`).
4. Add **Inbound Rules**:
   - **Type:** SSH
   - **Source:** My IP (or restrict to your IP address)
   - Add another rule for **HTTP**:
     - **Type:** HTTP
     - **Source:** 0.0.0.0/0 (to allow public access)
5. Click **Create security group**.

### Step 10: Launch EC2 Instance in VPC

1. Now that your VPC is set up, you can launch an EC2 instance.
2. Go to the **EC2 Dashboard** and click **Launch Instance**.
3. In the **Configure Instance** section, choose the VPC and subnet you just created.
4. Assign a security group to your instance (use the security group created in Step 9).

---

## Additional Resources
- [AWS VPC Documentation](https://docs.aws.amazon.com/vpc/index.html)
- [AWS Free Tier](https://aws.amazon.com/free/)
