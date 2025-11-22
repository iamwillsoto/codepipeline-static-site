# CI/CD Static Website with AWS CodePipeline, S3, CloudFront, and GitHub

This project turns a simple static website into an automated CI/CD pipeline with global distribution.

The same site is delivered in three ways:
1. GitHub → CodePipeline → S3 (auto-deploy on commit)
2. Pipeline with manual approval + email notification
3. CloudFront fronting S3, serving a Cloud Resume template

---

## Architecture

- GitHub repository (source of truth)
- AWS CodePipeline (CI/CD orchestration)
- S3 static website bucket (hosting)
- IAM roles (pipeline + S3 access)
- SNS topic (manual approval email)
- CloudFront distribution (CDN in front of S3)

High-level flow:

Developer commit → GitHub → CodePipeline trigger  
→ (optional) manual approval via SNS email  
→ deploy to S3 → served globally via CloudFront

---

## Implementation Summary

### 1) Base Pipeline: GitHub → S3

- Created a GitHub repo and uploaded `website.html`
- Created an S3 bucket with static website hosting enabled
- Configured CodePipeline:
  - Source: GitHub (Version 2)
  - Build: skipped (static HTML)
  - Deploy: S3 website bucket
- Confirmed that every commit to `main` automatically updated the S3-hosted site

### 2) Manual Approval + CloudFront

- Added an Approval stage in CodePipeline with an SNS topic
- Subscribed an email to receive approval requests before deploying to S3
- Created a CloudFront distribution pointing at the S3 static website endpoint
- Verified lower-latency access to the site via the CloudFront URL

### 3) Cloud Resume Deployment

- Replaced the basic HTML with a Cloud Resume-style template
- Committed the updated resume HTML to GitHub
- Approved the pipeline deployment via the email link
- Verified the resume was live at the CloudFront URL

---

## Technologies Used

- AWS S3 (static website hosting)
- AWS CodePipeline
- AWS IAM roles and policies
- AWS SNS (manual approval notifications)
- AWS CloudFront (CDN)
- GitHub (source control)
- HTML/CSS (static resume content)

---

## How to Reuse This Pattern

At a high level, you can adapt this setup for:

- Static marketing sites
- Documentation portals
- Personal portfolios or resumes
- Any read-only site that benefits from CI/CD and a global CDN

The key idea: connect GitHub to CodePipeline, deploy to S3, put CloudFront in front, and optionally gate production with a manual approval step.

---

## Repository

This repo contains:

- Static HTML for the initial site and resume
- README describing the architecture and flow
- (Optional) validation screenshots folder documenting the pipeline runs

