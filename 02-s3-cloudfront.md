# Add CloudFront to Your S3 Website

> **SV Technologies** · [← Back to README](./README.md)
> Everything done in the **AWS Console — no CLI needed.**

---

## Before You Start

Complete [S3 Static Hosting](./01-amazon-s3.md) first.
Your S3 website must already be live before adding CloudFront.

---

## Why Add CloudFront?

| Without CloudFront | With CloudFront |
|---|---|
| HTTP only (not secure) | HTTPS (padlock in browser) |
| Served from one region | 600+ locations worldwide |
| Slow for users far away | Fast everywhere |
| Long S3 URL | Custom domain supported |

---

## What is CloudFront?

CloudFront is a **CDN — Content Delivery Network**.

It copies your website to 600+ servers around the world.
When someone visits your site, they load it from the nearest server.

```
User in Mumbai  →  loads from Mumbai server  (fast)
User in London  →  loads from London server  (fast)
User in Sydney  →  loads from Sydney server  (fast)
```

All of those servers get the files from your one S3 bucket.

---

## Step 1 — Open CloudFront

1. In the AWS Console top search bar, type **`CloudFront`**
2. Click **CloudFront** under Services
3. Click **Create a CloudFront distribution**

---

## Step 2 — Set the Origin (Your S3 Bucket)

1. **Origin domain** → click the text box
   - A dropdown appears with your S3 buckets
   - Select your S3 bucket (e.g. `my-portfolio-2024.s3.ap-south-1.amazonaws.com`)
   - A yellow banner may appear — **do not** click "Use website endpoint"
     (keep the S3 REST endpoint, not the website endpoint)

2. **Origin access** → select **Origin access control settings (recommended)**

3. Under **Origin access control** → click **Create new OAC**
   - A dialog opens
   - Leave the name as default
   - Click **Create**
   - The OAC is now selected ✅

---

## Step 3 — Configure HTTPS

1. Scroll to **Viewer** section
2. **Viewer protocol policy** → select **Redirect HTTP to HTTPS**
   - This means anyone visiting `http://` will be automatically redirected to `https://`

---

## Step 4 — Set Default Root Object

1. Scroll all the way down to the **Settings** section
2. **Default root object** → type `index.html`
   - Without this, visiting your domain root shows an error instead of your homepage

---

## Step 5 — Create the Distribution

1. Click **Create distribution** at the bottom
2. A blue banner appears — this is important:
   > "You must update the S3 bucket policy"
3. Click **Copy policy** button in the banner
4. Keep this tab open — you will need the policy in the next step

---

## Step 6 — Update Your S3 Bucket Policy

You need to give CloudFront permission to read your S3 files.

1. Open a new tab → go to **S3** → open your bucket
2. Click the **Permissions** tab
3. Click **Bucket policy** → click **Edit**
4. **Replace** all existing text with the policy you copied in Step 5
5. Click **Save changes**

Your S3 bucket is now **private** — only CloudFront can access it.
Direct S3 URL access will be blocked (that's correct and secure).

---

## Step 7 — Wait for Deployment

1. Go back to your CloudFront distribution tab
2. The **Status** column shows **Deploying**
3. Wait 5–10 minutes until status changes to **Enabled** ✅

---

## Step 8 — Get Your CloudFront URL

1. Click on your distribution to open it
2. Find **Distribution domain name** — it looks like:
   ```
   d1abc123xyz.cloudfront.net
   ```
3. Open that URL in your browser
4. Your site now loads over **HTTPS** with a padlock! 🔒

---

## Adding a Custom Domain (Optional)

To use `www.yourname.com` instead of the CloudFront URL:

### Part A — Get a Free SSL Certificate

1. In AWS Console, search for **Certificate Manager** (ACM)
2. **Important:** Switch region to **us-east-1 (US East N. Virginia)**
   - CloudFront only works with certificates from us-east-1
3. Click **Request a certificate**
4. Select **Request a public certificate** → click **Next**
5. **Fully qualified domain name** → type your domain (e.g. `www.yourname.com`)
6. Click **Add another name to this certificate** → also add `yourname.com`
7. **Validation method** → select **DNS validation**
8. Click **Request**

### Part B — Validate Your Domain

1. Click on your pending certificate
2. You see DNS records to add — click **Export to CSV** or note the values
3. Go to your domain registrar (GoDaddy, Namecheap, etc.)
4. Add the **CNAME records** ACM shows you
5. Wait 5–30 minutes — certificate status changes to **Issued** ✅

### Part C — Add Domain to CloudFront

1. Go back to **CloudFront** → open your distribution → click **Edit**
2. **Alternate domain names (CNAMEs)** → click **Add item**
3. Type `www.yourname.com`
4. Type `yourname.com` (add a second one)
5. **Custom SSL certificate** → select the ACM certificate you just created
6. Click **Save changes** → wait for redeployment

### Part D — Update Your DNS

1. Go to your domain registrar
2. Add a new **CNAME record**:
   - **Name:** `www`
   - **Value:** your CloudFront domain (e.g. `d1abc123xyz.cloudfront.net`)
3. Wait for DNS propagation (up to 48 hours, usually under 1 hour)
4. Visit `www.yourname.com` — your site! 🎉

---

## Updating Your Website

After you upload new files to S3, CloudFront still serves the old cached version.
Force it to refresh:

1. Open your CloudFront distribution
2. Click the **Invalidations** tab
3. Click **Create invalidation**
4. In the **Add object paths** box, type: `/*`
   (the `*` means "clear everything")
5. Click **Create invalidation**
6. Wait 1–2 minutes — then refresh your site

---

## Common Problems

| Problem | What to check |
|---|---|
| 403 Access Denied | S3 bucket policy not updated — redo Step 6 |
| Site not loading | Wait 10 min for distribution to deploy fully |
| Old version showing | Create an invalidation (see above) |
| HTTPS not working | Check OAC was created and bucket policy updated |
| Custom domain 404 | Check CNAME record in your DNS registrar |

---

## Next Step

Auto-deploy from GitHub → [AWS Amplify](./03-aws-amplify.md)

---

*SV Technologies · sv-technologies.in*
