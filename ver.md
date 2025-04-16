Here’s how you can **step-by-step complete SET-2 tasks** on AWS involving S3:

---

## ✅ **1. Create an S3 Bucket Named `constrain`**

### 🔧 Using AWS Console:
1. Go to the **S3 Dashboard**
2. Click **“Create bucket”**
3. Bucket name: `constrain` (must be globally unique)
4. Choose your **Region**
5. Leave all other settings as default for now
6. Click **“Create bucket”**

---

## ✅ **2. Enable Public Access for the Bucket**

> ⚠️ Be cautious: Public access makes content accessible to everyone on the internet.

### Step-by-step:
1. Go to the `constrain` bucket
2. Click on **Permissions**
3. Scroll to **Block public access (bucket settings)**  
4. Click **Edit**, uncheck:
   - Block all public access
5. Confirm the warning and click **Save changes**
6. Then under **Permissions**, add a **Bucket policy** like this:

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "PublicReadGetObject",
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::constrain/*"
  }]
}
```

---

## ✅ **3. Enable Versioning for the Bucket**

### Steps:
1. Go to **S3 Dashboard > constrain bucket**
2. Click on **Properties**
3. Scroll to **Bucket Versioning**
4. Click **Edit**
5. Select **Enable**
6. Click **Save changes**

---

## ✅ **4. Display the Bucket's Metadata**

### Using AWS CLI:
```bash
aws s3api head-bucket --bucket constrain
```

To list the bucket metadata (like versioning):
```bash
aws s3api get-bucket-versioning --bucket constrain
```

To see full details:
```bash
aws s3api get-bucket-location --bucket constrain
aws s3api get-bucket-acl --bucket constrain
aws s3api get-bucket-policy --bucket constrain
```

---

## ✅ **5. Purpose and Benefits of Versioning in S3**

### 🔄 **Purpose:**
- **Versioning** allows you to keep **multiple versions of an object** in one bucket.
- Each update or overwrite creates a **new version**.
- You can **recover deleted or overwritten files** easily.

---

### 🌟 **Benefits:**
| Benefit                        | Description |
|-------------------------------|-------------|
| 🕒 **Accidental Recovery**     | Restore a previous version if a file is deleted or corrupted. |
| 🗃️ **Change Tracking**         | Keep history of changes to files. |
| 🛡️ **Data Protection**         | Protects against accidental deletes or overwrites. |
| 🔄 **Easy Rollback**           | Revert to a known good version if needed. |
| 🔍 **Audit and Compliance**    | Useful for maintaining logs and history for audit purposes. |

---

Let me know if you want to automate these steps using AWS CLI or Terraform!