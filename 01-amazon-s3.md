# Host a Static Website on Amazon S3

> **SV Technologies** · [← Back to README](./README.md)
> Everything done in the **AWS Console — no CLI needed.**

---

## What is Amazon S3?

S3 (Simple Storage Service) stores files in the cloud.
You can turn any S3 bucket into a live website with one setting.

> Upload your files → enable website hosting → get a public URL.

---

## Step 1 — Sign in to AWS Console

1. Go to **[console.aws.amazon.com](https://console.aws.amazon.com)**
2. Enter your **email** and **password**
3. You land on the AWS Console home page

---

## Step 2 — Open S3

1. In the top search bar, type **`S3`**
2. Click **S3** under Services
3. You see the S3 dashboard — click **Create bucket**

---

## Step 3 — Create a Bucket

1. **Bucket name** → type a unique name (e.g. `my-portfolio-2024`)
   > Bucket names must be globally unique across all AWS accounts
2. **AWS Region** → choose the region closest to your users
   (e.g. `ap-south-1` for India)
3. **Object Ownership** → leave as default
4. **Block Public Access settings** →
   - **Uncheck** the box that says "Block all public access"
   - A warning appears — check the box that says "I acknowledge..."
5. Everything else → leave as default
6. Click **Create bucket** at the bottom

---

## Step 4 — Upload Your Website Files

1. Click your bucket name to open it
2. Click the **Upload** button
3. Click **Add files**
4. Select your `index.html`, `style.css`, `script.js` files
   (also add any images or folders you have)
5. Click **Upload** at the bottom
6. Wait for the green success banner — then click **Close**

---

## Step 5 — Make Files Public

Your files are private by default. You need a **bucket policy** to make them public.

1. Click the **Permissions** tab of your bucket
2. Scroll down to **Bucket policy** → click **Edit**
3. Delete any existing text in the editor
4. Paste the following — **replace `YOUR-BUCKET-NAME`** with your actual bucket name:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
    }
  ]
}
```

5. Click **Save changes**
6. You will see a red "Publicly accessible" badge — that is correct ✅

---

## Step 6 — Enable Static Website Hosting

1. Click the **Properties** tab of your bucket
2. Scroll all the way down to **Static website hosting**
3. Click **Edit**
4. **Static website hosting** → select **Enable**
5. **Hosting type** → select **Host a static website**
6. **Index document** → type `index.html`
7. **Error document** → type `error.html` (optional)
8. Click **Save changes**

---

## Step 7 — Get Your Website URL

1. Click the **Properties** tab again
2. Scroll down to **Static website hosting**
3. You will see a **Bucket website endpoint** link like:
   ```
   http://my-portfolio-2024.s3-website.ap-south-1.amazonaws.com
   ```
4. Click the link — your website is live! 🎉

---

## Updating Your Website

When you change your files and want to update the live site:

1. Go to your S3 bucket → click **Upload**
2. Upload the new version of your files
3. Click **Upload** → wait for success
4. Refresh your website URL — changes are live immediately

---

## What the URL Looks Like

```
http://[bucket-name].s3-website.[region].amazonaws.com
```

Example:
```
http://my-portfolio-2024.s3-website.ap-south-1.amazonaws.com
```

> **Note:** This URL uses HTTP, not HTTPS.
> To get HTTPS + a custom domain, follow the [CloudFront guide](./02-s3-cloudfront.md).

---

## Common Problems

| Problem | What to check |
|---|---|
| 403 Forbidden | Bucket policy not saved correctly — redo Step 5 |
| 404 Not Found | Index document not set — redo Step 6 |
| Files not loading | Images/CSS path is wrong in your HTML file |
| Old version showing | Your browser cached the old version — press Ctrl+Shift+R |

---

## Next Step

Add **HTTPS and global speed** → [S3 + CloudFront](./02-s3-cloudfront.md)

---

*SV Technologies · sv-technologies.in*
