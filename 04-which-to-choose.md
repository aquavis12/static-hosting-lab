# Which Method Should I Use?

> **SV Technologies** · [← Back to README](./README.md)

---

## Quick Decision

```
Are you brand new to AWS?
├── YES → Start with S3 (learn the basics first)
└── NO  →
        Do you update your site regularly?
        ├── YES → Use Amplify (auto-deploys on every push)
        └── NO  →
                Do you need HTTPS + fast global loading?
                ├── YES → Use S3 + CloudFront
                └── NO  → S3 alone is enough
```

---

## Side-by-Side Comparison

| | S3 | S3 + CloudFront | Amplify |
|---|---|---|---|
| **Setup time** | 10 min | 25 min | 5 min |
| **HTTPS** | ❌ | ✅ | ✅ |
| **Global speed** | ❌ | ✅ | ✅ |
| **Custom domain** | ✅ (hard) | ✅ (medium) | ✅ (easy) |
| **Auto-deploy** | ❌ | ❌ | ✅ |
| **Branch previews** | ❌ | ❌ | ✅ |
| **Good for learning AWS** | ✅✅ | ✅✅✅ | ✅ |
| **Cost** | ~Free | ~Free | Free tier |

---

## By Situation

### "I just want to host my portfolio quickly"
→ **AWS Amplify** — connect GitHub, done in 5 minutes

### "I want to understand how AWS works"
→ **S3 + CloudFront** — you configure everything manually, you learn more

### "I update my site every week"
→ **AWS Amplify** — auto-deploys so you never need to upload files again

### "I want the absolute cheapest option"
→ **S3 alone** — a few cents per month, or free with the AWS free tier

### "I'm preparing for an AWS certification"
→ **S3 + CloudFront** — these are core services covered in every AWS exam

### "I'm building a team project"
→ **Amplify** — branch previews let every teammate test changes before merge

---

## What Most Beginners Do

1. **Start with S3** — learn how buckets, permissions, and policies work
2. **Add CloudFront** — understand CDNs and HTTPS
3. **Move to Amplify** — for real projects where you push code often

That path teaches you the most and sets you up for real AWS jobs.

---

## Cost Reality

For a typical student portfolio site:

| Method | Monthly Cost |
|---|---|
| S3 alone | $0.00 (within free tier) |
| S3 + CloudFront | $0.00 (within free tier) |
| Amplify | $0.00 (within free tier) |

All three are **free** for a simple portfolio during the 12-month AWS free tier.
After 12 months, all three cost under **$1/month** for a normal portfolio.

---

## The One Mistake to Avoid

Do not leave an S3 bucket **publicly accessible** without a website use case.
If you finish the workshop, either:
- Keep it running (it's basically free)
- Or delete the bucket: S3 → select bucket → **Delete**

Same for CloudFront distributions — delete them when you are done learning.

---

*SV Technologies · sv-technologies.in*
