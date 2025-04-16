Here's your guide for **Set 5: EBS & SRR (Same Region Replication)** — explained step by step using AWS Console.

---

## ✅ **Part 1: EBS (Elastic Block Store)**

### 🔧 Goal:
Attach a persistent block storage (EBS) volume to an EC2 instance.

---

### 📍 **Step-by-Step: Create and Attach EBS Volume**

#### ✅ Step 1: Launch an EC2 Instance (if not already done)
1. Go to **EC2 Dashboard → Launch Instance**
2. Choose an **Amazon Linux** or **Ubuntu** AMI
3. Select instance type: `t2.micro` (Free tier)
4. Configure VPC/subnet/security group
5. Launch with key pair

---

#### ✅ Step 2: Create an EBS Volume
1. Go to **EC2 → Volumes**
2. Click **Create Volume**
3. Volume type: `gp2` or `gp3` (General Purpose SSD)
4. Size: e.g., `8 GiB`
5. Availability Zone: Must match EC2’s AZ (e.g., `us-east-1a`)
6. Click **Create Volume**

---

#### ✅ Step 3: Attach EBS Volume to EC2
1. Select the newly created volume
2. Click **Actions → Attach Volume**
3. Choose the instance you launched
4. Device name: `/dev/xvdf` (default is fine)
5. Click **Attach**

---

#### ✅ Step 4: Mount EBS on EC2
SSH into your instance:

```bash
sudo lsblk       # To see the new volume (likely /dev/xvdf)
sudo mkfs -t ext4 /dev/xvdf     # Format the volume
sudo mkdir /data
sudo mount /dev/xvdf /data
```

To persist after reboot, add to `/etc/fstab`:

```bash
echo '/dev/xvdf /data ext4 defaults,nofail 0 2' | sudo tee -a /etc/fstab
```

---

## ✅ **Part 2: SRR (Same Region Replication)**

### 🔁 Goal:
Replicate objects **between two S3 buckets in the same region**.

---

### 📍 **Step-by-Step: Set Up SRR**

#### ✅ Step 1: Create Source & Destination Buckets
1. Go to **S3 → Create bucket**
   - Name: `srr-source-bucket-<yourname>`
   - Region: e.g., `us-east-1`
   - **Enable versioning**
2. Create another bucket:
   - Name: `srr-destination-bucket-<yourname>`
   - Same region
   - **Enable versioning**

---

#### ✅ Step 2: Create an IAM Role for SRR
1. Go to **IAM → Roles → Create Role**
2. Use **S3 → S3 replication**
3. Attach default S3 replication policy
4. Name: `s3-srr-role`
5. Click **Create Role**

---

#### ✅ Step 3: Enable Replication Rule (SRR)
1. Go to **Source bucket → Management tab**
2. Click **Create replication rule**
3. Name: `replicate-to-destination`
4. Enable rule for **entire bucket**
5. Destination bucket: `srr-destination-bucket-<yourname>`
6. Choose the role: `s3-srr-role`
7. Save

---

#### ✅ Step 4: Test SRR
1. Upload an object to the **source bucket**
2. It will appear automatically in the **destination bucket** within the same region

