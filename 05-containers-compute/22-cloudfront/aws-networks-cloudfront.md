<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Website Delivery with CloudFront

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-cloudfront)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

![Image](https://learn.nextwork.org/projects/static/aws-networks-cloudfront/three-tier.png)

---

## Website Delivery with CloudFront

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-cloudfront_1dddddwe)

---

## Introducing Today's Project!

In this project, I will demonstrate :

🪣 Create a storage space in S3 for the website's files.

🌐 Set up CloudFront to distribute the website globally.

🔑 Manage permissions for both S3 and CloudFront.

💎 Compare different methods for hosting the website and analyze their performance.

### Tools and concepts

In this project I learned about Amazon S3, which is used to store website files, and Amazon CloudFront, which is a CDN that distributes content globally through edge locations for faster delivery. I also learned about Origin Access Control (OAC), which keeps the S3 bucket private while allowing only CloudFront to access it, bucket policies, which control who can read files in S3, and the difference between CloudFront and S3 static website hosting in terms of security and performance.

### Project reflection

This project took me approximately 1 hour

I chose to do this project today because I wanted to learn how content delivery networks work and how to host a website on AWS. Something that would make learning with NextWork even better is having more visual diagrams to illustrate how the different services interact with each other.

---

## Set Up S3 and Website Files

I started the project by creating an S3 bucket to store the website's files , I can't use CloudFront for this task because it's a content delivery content tool that's used only to destribute the website globally and optimize its performance.

The three files that make up my website are index.html, which is the main file that defines the structure of the website , style.css, which is a file that adds style and consistent appearance to the website and script.js, which is a file that adds interactivity .

I validated that my website files work by opening them in the browser and verifying that everything functions correctly.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-cloudfront_qgo7wcd3)

---

## Exploring Amazon CloudFront

Amazon CloudFront is a content delivery network (CDN). It is a service that optimizes a website's performance by caching content on multiple servers around the world, called edge locations. Whenever a user makes a request, it is automatically routed to the nearest edge location to reduce latency and improve speed.

To use Amazon CloudFront, you set up distributions, which are set of rules that tells CloudFront how to deliver the content to user.

I created a CloudFront distribution from the AWS console, selected the single website option, and configured my S3 bucket as the origin. I kept the default caching and security settings, did not enable WAF, and deployed the distribution

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-cloudfront_qgo7wcdt)

---

## Handling Access Issues

When I tried visiting my distributed website, I ran into an access denied error because CloudFront did not have the proper permissions to access the objects inside my S3 bucket.

My distribution's origin access settings were set to Origin Access Control (OAC) - recommended, meaning only CloudFront is allowed to access the S3 bucket privately. This caused the Access Denied error because even though CloudFront was given permission to access the bucket, the S3 bucket policy was not yet updated to trust and recognize CloudFront as an authorized accessor. CloudFront was essentially showing up with its VIP badge, but the S3 bucket's security had no guest list confirming it — so S3 denied the request. The fix is to add the bucket policy in S3 that explicitly allows this specific CloudFront distribution to read the objects

To resolve the error, I set up Origin Access Control (OAC). OAC is a special permission that allows only CloudFront to access the S3 bucket and its objects privately, meaning the bucket and its contents are not publicly accessible to anyone else on the internet — only CloudFront can read and serve them.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-cloudfront_egrhntyu)

---

## Updating S3 Permissions

Once I set up my OAC, I still needed to update my bucket policy because the s3 bucket still needs to explictly grant the OAC permission to the bucket's content.

Creating an OAC automatically gives me a policy I could copy, which grants only CloudFront (cloudfront.amazonaws.com) permission to perform the s3:GetObject action — meaning it can read and retrieve objects from my S3 bucket (nextwork-three-tier-ayoub-199). The policy also includes a condition that restricts this access to only my specific CloudFront distribution (E2MASM8MUNDMES), so no other CloudFront distribution can access my bucket either. This policy is what gets added to the S3 bucket so that S3 knows to trust and allow requests coming from my CloudFront distribution

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-cloudfront_eg98ntyu)

---

## S3 vs CloudFront for Hosting

For my project extension, I compared two different methods of hosting a website on AWS. The first method used CloudFront as the delivery layer, and the second accessed the website directly through S3's static website hosting feature.

When testing the S3 static website hosting method, I initially ran into an Access Denied error. The cause was that my bucket policy only granted read access to my specific CloudFront distribution, meaning all other requests — including direct browser access through the S3 website endpoint — were being blocked

I tried resolving this by unchecking the "Block public access (bucket settings)" option, but I still ran into an error. This is because unchecking that setting alone isn't enough — it simply removes the restriction that prevents public access, but it doesn't actually grant it. To fully enable public access, I also needed to add a bucket policy that explicitly allows anyone to read the files in the bucket.

I could finally see my S3 hosted website when I added a bucket policy that grants public read access to all files in the bucket. This worked because the policy explicitly tells S3 to allow anyone to retrieve the files stored in the bucket, which is exactly what static website hosting needs in order to serve content directly to visitors' browsers.

Compared to CloudFront, using S3 static website hosting meant making the bucket completely public, which is less secure. With CloudFront, the bucket stayed private and only accessible through the OAC. I preferred the CloudFront approach as it offers better security while also delivering the website faster through edge locations.

---

## S3 vs CloudFront Load Times

Load time is the amount of time it takes for a website to fully appear in my browser. The load times for the CloudFront site were faster than the S3 site because CloudFront uses edge locations near the user to deliver content, whereas S3 serves files from a single region that may be far away from the user.

A business would prefer CloudFront when it wants to deliver its services globally to users across different locations, while S3 static website hosting might be sufficient when the target audience is limited to a single region.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-cloudfront_12verpuh)

---

---
