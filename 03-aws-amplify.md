# Deploy with AWS Amplify

> **SV Technologies** · [← Back to README](./README.md)
> Everything done in the **AWS Console — no CLI needed.**

---

## What is AWS Amplify?

Amplify connects to your GitHub repository and deploys your site automatically.

> Push code to GitHub → Amplify detects it → builds and deploys → live in 60 seconds.

No manual uploads. No clicking around after setup.

---

## How It Works

```
You edit your files on your computer
          ↓
You push to GitHub  (git push)
          ↓
Amplify sees the push automatically
          ↓
Amplify builds your site
          ↓
New version is live — same URL, no downtime
```

---

## Before You Start

You need:
- [ ] Your website files pushed to a **GitHub repository**
- [ ] An **AWS account**

If your files aren't on GitHub yet:
1. Go to [github.com](https://github.com) → click **New repository**
2. Name it (e.g. `my-portfolio`)
3. Upload your files using the GitHub web interface (drag and drop)
4. Click **Commit changes**

---

## Step 1 — Open AWS Amplify

1. Go to **[console.aws.amazon.com](https://console.aws.amazon.com)**
2. In the search bar, type **`Amplify`**
3. Click **AWS Amplify** under Services
4. You land on the Amplify home page

---

## Step 2 — Create a New App

1. Click **Create new app**
2. You see "Deploy your app" — click **GitHub**
3. Click **Next**

---

## Step 3 — Authorise GitHub

1. A GitHub popup window opens
2. Click **Authorize AWS Amplify**
3. If asked, enter your GitHub password to confirm
4. The popup closes — you are now back in the AWS Console

---

## Step 4 — Select Your Repository

1. **GitHub account** → your GitHub username should appear
2. **Repository** → click the dropdown and select your repo
   (e.g. `my-portfolio`)
3. **Branch** → select `main` (or `master` — whichever your repo uses)
4. Click **Next**

---

## Step 5 — Review Build Settings

Amplify automatically detects what kind of site you have.

**For a plain HTML/CSS/JS site:**
You will see a build spec like this — you can leave it as-is:

```yaml
version: 1
frontend:
  phases:
    build:
      commands: []
  artifacts:
    baseDirectory: /
    files:
      - '**/*'
```

**For a React app:**
Amplify fills in the build command automatically:

```yaml
version: 1
frontend:
  phases:
    preBuild:
      commands:
        - npm install
    build:
      commands:
        - npm run build
  artifacts:
    baseDirectory: build
    files:
      - '**/*'
```

Click **Next**.

---

## Step 6 — Deploy

1. Review the summary — check the repo, branch, and build settings
2. Click **Save and deploy**
3. Watch the pipeline run — you see 4 stages:
   - **Provision** — Amplify sets up a build environment
   - **Build** — your code is built
   - **Deploy** — files are pushed to the CDN
   - **Verify** — Amplify checks the deployment
4. All 4 stages turn green ✅
5. Your site is live!

---

## Step 7 — Find Your URL

1. In the Amplify console, look for your app
2. Under the branch name (e.g. `main`) you see a URL like:
   ```
   https://main.d1abc123.amplifyapp.com
   ```
3. Click the URL — your website opens in a new tab 🎉

---

## How Auto-Deploy Works Going Forward

Now whenever you update your site:

1. Edit your files on your computer
2. Push to GitHub (or edit files directly on GitHub.com)
3. Go to AWS Amplify — you will see a new deployment start automatically
4. Wait ~1 minute — your site is updated with zero downtime

You never need to touch the AWS Console again for regular updates.

---

## Adding a Custom Domain

1. In Amplify → open your app → click **Domain management** in the left menu
2. Click **Add domain**
3. Type your domain name (e.g. `yourname.com`)
4. Click **Configure domain**
5. Amplify shows you DNS records to add
6. Go to your domain registrar → add those records
7. Come back and click **Save**
8. Amplify provisions an SSL certificate automatically
9. Wait 24 hours for DNS to propagate — then your custom domain is live ✅

---

## Branch Preview URLs

Every branch in your repo gets its own preview URL:

| Branch | URL |
|---|---|
| `main` | `https://main.d1abc123.amplifyapp.com` |
| `feature-redesign` | `https://feature-redesign.d1abc123.amplifyapp.com` |
| `staging` | `https://staging.d1abc123.amplifyapp.com` |

This lets you test changes before merging to main.

To add a new branch for preview:
1. Amplify → your app → click **Add branch**
2. Select the branch from the dropdown
3. Click **Save** — it deploys that branch automatically

---

## Rollback to a Previous Version

If a deployment breaks something:

1. Amplify → your app → click your branch (e.g. `main`)
2. You see a list of all past deployments
3. Click on a previous successful deployment
4. Click **Redeploy this version**
5. Your site reverts in about 1 minute

---

## Common Problems

| Problem | What to check |
|---|---|
| Build fails | Click on the failed deployment → scroll to the build log → read the error |
| Site shows old version | Check if deployment finished — look for all-green stages |
| GitHub not connecting | Click "Disconnect and reconnect" in Amplify → reauthorise GitHub |
| Custom domain not working | DNS propagation takes time — wait up to 48 hours |
| Wrong folder deployed | Check `baseDirectory` in build settings matches your output folder |

---

## Amplify vs S3 — When to Use Each

| | S3 + CloudFront | Amplify |
|---|---|---|
| You update your site often | ❌ Manual upload each time | ✅ Auto on every push |
| You want to understand AWS deeply | ✅ Teaches you more | ❌ Hides the details |
| You have a team / use GitHub | ❌ No branch previews | ✅ Preview every branch |
| Cheapest possible | ✅ Pennies/month | ✅ Free tier covers most sites |
| Simplest setup | ❌ More steps | ✅ 5 minute setup |

---

*SV Technologies · sv-technologies.in*
