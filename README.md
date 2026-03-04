# AWS Project 1 — Static Website Hosting using S3 and CloudFront

This project demonstrates how to host a static website on AWS using **Amazon S3** and accelerate it globally using **Amazon CloudFront CDN**.

The goal of this project is to understand how static websites are deployed in production using cloud infrastructure.

---

# Architecture

User → CloudFront CDN → S3 Bucket (Static Website)
https://d195f3juzt5pmc.cloudfront.net/

CloudFront caches content at edge locations to reduce latency and improve performance for global users.

---

# Technologies Used

* AWS S3 (Static Website Hosting)
* AWS CloudFront (Content Delivery Network)
* AWS IAM (Access Management)
* HTML & CSS

---

# Project Steps

## 1. Created a Static Website

A simple website was created with the following files:

index.html
style.css

Example structure:

```
aws-s3-project/
 ├── index.html
 └── style.css
```

---

## 2. Created an S3 Bucket

Steps:

1. Open AWS Console
2. Navigate to **S3**
3. Click **Create Bucket**
4. Enter a globally unique bucket name
5. Choose region **ap-south-1 (Mumbai)**
6. Disable **Block Public Access**

Example bucket name:

```
sahil-static-site
```

---

## 3. Uploaded Website Files

Uploaded the following files to the bucket:

* index.html
* style.css

These files are stored as **objects** inside the S3 bucket.

---

## 4. Enabled Static Website Hosting

Steps:

S3 → Bucket → Properties → Static Website Hosting → Enable

Configuration:

```
Index document: index.html
```

AWS generated a website endpoint such as:

```
http://bucket-name.s3-website-ap-south-1.amazonaws.com
```

At this point the website showed **Access Denied** because S3 objects are private by default.

---

## 5. Added Bucket Policy for Public Access

To allow users to read files from the bucket, the following bucket policy was added.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicRead",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::bucket-name/*"
    }
  ]
}
```

This policy allows anyone on the internet to read files from the bucket.

After this change, the S3 website became publicly accessible.

---

# Adding CloudFront CDN

To improve performance and enable global caching, **Amazon CloudFront** was added in front of the S3 website.

---

## 6. Created CloudFront Distribution

Steps:

1. Navigate to **CloudFront**
2. Click **Create Distribution**
3. Set origin to the **S3 website endpoint**
4. Enable **Redirect HTTP to HTTPS**
5. Set **Default Root Object = index.html**
6. Leave alternate domain empty
7. Skip AWS WAF for this project

Deployment takes approximately **5–10 minutes**.

---

## 7. Access the Website via CloudFront

CloudFront generates a domain such as:

```
https://dxxxxxxx.cloudfront.net
```

Users accessing this URL receive cached content from the nearest AWS edge location.

---

# Verifying CDN Caching

You can verify CloudFront caching using:

```
curl -I https://your-cloudfront-url
```

Check the response header:

```
x-cache: Hit from cloudfront
```

or

```
x-cache: Miss from cloudfront
```

This indicates whether the response was served from cache.

---

# Final Architecture

```
User
  ↓
CloudFront CDN
  ↓
S3 Static Website
  ↓
index.html / css / assets
```

---

# Key Learnings

* Understanding **S3 bucket structure**
* Configuring **static website hosting**
* Writing **S3 bucket policies**
* Deploying **CloudFront CDN**
* Understanding **edge caching**
* Learning how AWS serves static websites globally

---

# Future Improvements

* Use **Route 53** to attach a custom domain
* Add **HTTPS certificate using AWS ACM**
* Make S3 bucket **private using CloudFront Origin Access Control**
* Enable **CloudFront cache invalidation**
* Add **CI/CD deployment**

---

# Author

Sahil Kumar
Full Stack / Backend Engineer
