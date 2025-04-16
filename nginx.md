Here’s a **step-by-step guide** to Set 6: **Setting up Private and Public EC2 Servers** and installing **NGINX** on both.

---

## ✅ **Goal**
- Create one **Public EC2 instance** (accessible from internet)
- Create one **Private EC2 instance** (only accessible via Public instance)
- Install **NGINX** on both

---

## 🛠️ **STEP 1: Create a VPC**
1. Go to **VPC Dashboard → Create VPC**
2. Name: `custom-vpc`
3. Choose: **VPC only**
4. CIDR block: `10.0.0.0/16`
5. Click **Create VPC**

---

## 🛠️ **STEP 2: Create 2 Subnets**
1. **Public Subnet**
   - Name: `public-subnet`
   - CIDR block: `10.0.1.0/24`
   - Enable **Auto-assign public IP**

2. **Private Subnet**
   - Name: `private-subnet`
   - CIDR block: `10.0.2.0/24`

---

## 🛠️ **STEP 3: Create Internet Gateway**
1. Go to **Internet Gateways → Create**
2. Name: `my-igw`
3. Attach it to your VPC

---

## 🛠️ **STEP 4: Create Route Table for Public Subnet**
1. Go to **Route Tables → Create**
2. Name: `public-rt`
3. Attach to VPC
4. Add route:
   - Destination: `0.0.0.0/0`
   - Target: your **Internet Gateway**
5. Associate it with `public-subnet`

---

## 🛠️ **STEP 5: Create EC2 Instances**

### 🔓 Public EC2:
1. Launch instance in `public-subnet`
2. AMI: **Ubuntu** (or Amazon Linux)
3. Allow SSH (port 22) and HTTP (port 80) from `0.0.0.0/0`
4. Key pair: select/create one

### 🔐 Private EC2:
1. Launch instance in `private-subnet`
2. Same AMI
3. Do **not assign public IP**
4. Security group:
   - Allow SSH **only from public instance’s security group**
   - Allow HTTP from anywhere or internal IPs

---

## 🛠️ **STEP 6: Connect to Both Instances**

### ✅ Public Server:
Use your terminal or EC2 connect:

```bash
ssh -i "your-key.pem" ubuntu@<public-ec2-public-ip>
```

### ✅ Private Server (via Public):
1. SSH into Public EC2 first
2. Then from public EC2, connect to private EC2:

```bash
ssh -i your-key.pem ubuntu@10.0.2.x
```

(Use private IP of the private EC2)

---

## 🛠️ **STEP 7: Install NGINX on Both**

### 🟢 On Both Public & Private:
Run these commands after connecting:

```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

Check:
- For **Public EC2**, open browser → `http://<public-ip>` → NGINX default page appears
- For **Private EC2**, you can `curl` it from the public server:

```bash
curl http://10.0.2.x
```

---

## 🔐 Summary of Access

| Instance | Access From | Public IP | SSH | HTTP |
|----------|-------------|-----------|-----|------|
| Public   | Internet     | ✅        | ✅  | ✅   |
| Private  | Public EC2   | ❌        | ✅ (via public EC2) | Internal Only |

---

Want me to help you **automate this with a bash script or Terraform**?