<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Multi-Cloud Data Transfer with AWS and GCP

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-multicloud-storage)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

![Image](https://learn.nextwork.org/projects/static/aws-multicloud-storage/architecture-complete.png)

---

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-multicloud-storage_s5k4l5m6)

---

## Introducing Today's Project!

In this project, I will :

🪣 Set up and configure AWS S3 and GCP Cloud Storage buckets.

🔑 Use AWS IAM roles to connect GCP with AWS.

🚚 Troubleshoot the data transfer.

💎 Implement real-time data transfers using Amazon SQS.

### Tools and concepts

In this project, I learned to transfer data between AWS S3 and GCP Cloud Storage using GCP’s Storage Transfer Service. I worked with IAM roles, identity federation, and manifest files to securely and selectively move data. This taught me key multi-cloud concepts like cross-cloud access, data transfer scheduling, and storage management.

### Project reflection

This project took me approximately 60 min

---

## Setting up Data in S3

In this step, I set up an Amazon S3 bucket with a unique name. I kept the default settings and successfully created the bucket. After that, I uploaded 4 files from my computer into the bucket to store data in the cloud.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-multicloud-storage_s1g7h8j9)

---

## Setting up GCP

---

## Storage Transfer

Data transfers are important for migrating data between platforms, creating backups, and keeping data available in different environments. Using multiple cloud providers also gives benefits like better reliability, avoiding vendor lock-in, and more flexibility in managing costs and services. and our my case we are creating a transfer job to move our data from AWS S3 to Google Cloud Storage automatically using Google Cloud Storage Transfer Service.

The purpose of using Google Cloud Storage Transfer Service is to securely and efficiently move data between cloud platforms, such as from Amazon S3 to Google Cloud Storage. It handles authentication, manages the transfer process, and ensures the data moves correctly without needing to download and re-upload files manually. This makes the transfer faster, more reliable, and less error-prone.

Batch transfers are one-time or scheduled data transfers, usually used for large migrations or regular backups. They run at a specific time or on a schedule.

Event-driven transfers, on the other hand, automatically start whenever new files are added or updated in the source bucket. This makes them better for real-time synchronization, but they require more setup compared to batch transfers.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-multicloud-storage_s3k2l3m4)

---

## Granting GCP Access to AWS

To connect AWS and GCP, I'm using identity federation, which works by creating a trust relationship between the two cloud providers. I create an IAM role in AWS with permission to access S3, and then Google Cloud’s Storage Transfer Service is allowed to assume that role. When GCP proves its identity to AWS, AWS provides temporary credentials so it can access the S3 bucket.

This is more secure than other methods because it does not require sharing permanent access keys. The credentials are short-lived (they expire automatically after a short time), which reduces the risk of unauthorized access.

I needed to create a custom IAM role to securely allow my GCP project’s Storage Transfer Service to access my S3 bucket. The role grants read-only permissions and includes a custom trust policy so that only my specific GCP project (using identity federation and the subject ID) can assume the role. This ensures secure, controlled access between AWS and GCP as part of the multi-cloud project.

The subject ID is essential for this project because it uniquely identifies the specific Google Cloud Storage Transfer service account in your GCP project. It tells AWS exactly which external identity is allowed to assume the IAM role.

Without the subject ID, AWS would not know whether the request is coming from your legitimate GCP project or from someone else’s. By including the subject ID in the trust policy, we make sure that only your project’s Storage Transfer Service can access the S3 bucket, which keeps the connection secure and prevents unauthorized access.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-multicloud-storage_s4k3l4m5)

---

## Transferring from S3 to GCS!

The bucket was created using the Region location type, meaning the data is stored in a single geographic region close to my S3 bucket to improve transfer speed and keep costs lower.

For the storage class, I selected Standard, which is designed for frequently accessed data and provides high performance and availability.

I verified that my data transfer was successful by checking my bucket in Google Cloud Storage and confirming that it was created correctly. I also made sure that all the files from my Amazon S3 bucket were successfully transferred and are now present in the GCP bucket.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-multicloud-storage_s5k4l5m6)

---

## Transfer with a Manifest

I learned that a manifest file is like a detailed list for a data transfer. It lets you specify exactly which files to move instead of transferring everything in a bucket, which is especially useful for large datasets. The file must start with the header TsvHttpData-1.0, which tells Google Cloud’s Storage Transfer Service that the manifest is properly formatted. After the header, you list the exact file names you want to transfer.

I verified that my data transfer was successful by checking my bucket in Google Cloud Storage and confirming that it was created correctly. I also made sure that all the files from my Amazon S3 bucket were successfully transferred and are now present in the GCP bucket.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-multicloud-storage_rththrthrth)
