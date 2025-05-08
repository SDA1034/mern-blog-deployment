  sara-1034
  
#  Week-10 Assignment - MERN Stack Blog App Deployment on AWS

## Project Overview

This assignment demonstrates the full deployment of a MERN stack blog application using AWS services (EC2, S3, IAM) and MongoDB Atlas.


## Deployment URLs

- **Frontend (S3 Static Website):**  
  [http://sara-blog-frontend.s3-website.eu-north-1.amazonaws.com]

- **Backend (EC2 Public IP):**  
  `http://16.171.235.30:5000/api`

- **Media Bucket Base URL:**  
  `https://sara-blog-media.s3.eu-north-1.amazonaws.com`

---

## Technologies & Services Used

- React (Vite)
- Express.js
- AWS EC2 (Ubuntu 22.04)
- AWS S3 (Frontend + Media)
- AWS IAM
- PM2
- AWS CLI



## Setup & Deployment Steps

### 1. S3 Bucket for Frontend
- Bucket: `sara-blog-frontend`
- Enabled static website hosting
- Disabled "Block all public access"
- Applied public bucket policy
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "PublicReadGetObject",
			"Effect": "Allow",
			"Principal": "*",
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::sara-blog-frontend/*"
		}
	]
}

---------------------
### 2. S3 Bucket for Media
- Bucket: `sara-blog-media`
- Disabled "Block all public access"
- Configured CORS:
```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET", "PUT", "POST", "DELETE", "HEAD"],
    "AllowedOrigins": ["*"],
    "ExposeHeaders": ["ETag"]
  }
]

----------------------

# IAM User & Policy
- Created IAM user blog-user
- Attached custom policy allowing access to both buckets
  {
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"s3:PutObject",
				"s3:GetObject",
				"s3:DeleteObject",
				"s3:ListBucket"
			],
			"Resource": [
				"arn:aws:s3:::sara-blog-media",
				"arn:aws:s3:::sara-blog-media/*",
				"arn:aws:s3:::sara-blog-frontend",
				"arn:aws:s3:::sara-blog-frontend/*"
			]
		}
	]
}
- Create access key and save it
-------------------
# EC2 Setup
-Launched t3.mediume instance in eu-north-1
-Opened ports: 22, 80, 443, 5000
-Installed Node.js (via NVM), PM2, AWS CLI
-Cloned project and configured .env files
-Started backend using PM2
