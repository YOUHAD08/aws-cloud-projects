<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Encrypt Data with AWS KMS

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-kms)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

![Image](https://learn.nextwork.org/projects/static/aws-security-kms/architecture-complete.png)

---

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-kms_w0x1y2z3)

---

## Introducing Today's Project!

In this step, I will demonstrate how to secure data using AWS encryption services.

The goal is to:

🔑 Create encryption keys using AWS KMS (Key Management Service).

🗄️ Encrypt a DynamoDB database with a KMS key.

➕ Add and retrieve data from the database to test the encryption.

🕵️‍♀️ Observe how AWS prevents unauthorized access to the encrypted data.

💎 Grant a user permission to use the encryption key securely.

This step will help me understand how encryption and access control work together to protect data in AWS.

### Tools and concepts

In this project, I learned key AWS services like KMS for encryption, DynamoDB for data storage, and IAM for access control. I also understood important concepts such as data encryption, user permissions, and combining access control with encryption for layered security.

### Project reflection

This project took me approximately 60 min

I chose to do this project today because I wanted to understand how AWS handles data security and access control in practice. Something that would make learning with NextWork even better is having more hands-on examples that combine multiple AWS services in one workflow.

---

## Encryption and KMS

Encryption is the process of transforming normal data into unreadable data that is difficult to understand.

Companies and developers use encryption to secure and protect their data from being stolen or accessed by unauthorized users.

Encryption keys are the main tools used in this process. They tell the encryption algorithm how to encrypt and decrypt the data, ensuring that only authorized users can read it.

AWS KMS (Key Management Service) is a service created by Amazon Web Services that acts like a secure vault for encryption keys.

It allows you to create, store, and manage encryption keys while controlling who can access and use them.

AWS KMS also provides logging and monitoring of key usage, helping companies meet compliance requirements and maintain strong data security standards.

Encryption keys are broadly categorized into symmetric and asymmetric keys.

Symmetric keys are more efficient and faster for encrypting large amounts of data because they use the same key for both encryption and decryption.

That’s why I chose to set up a symmetric key, since I am encrypting a Amazon DynamoDB table. It is the best option for database encryption because it is faster and designed for high-performance workloads.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-kms_a2b3c4d5)

---

## Encrypting Data

Amazon DynamoDB is a NoSQL database known for being fast and flexible.

It is mainly used to store and access large amounts of data quickly, making it ideal for applications that require high performance and low latency.

Amazon DynamoDB offers three main encryption options:

1️⃣ AWS owned key – DynamoDB fully manages the encryption keys for you.

2️⃣ AWS managed key – The key is stored in your account but managed automatically by AWS Key Management Service (KMS).

3️⃣ Customer managed key – The key is stored in your account and fully managed by you. You control permissions, rotation, and access policies.

The main difference between these options is who manages the key and who has control over access to it.

I selected the customer managed key option because it provides the highest level of security and control.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-kms_q8r9s0t1)

---

## Data Visibility

Rather than controlling who can access the key itself, KMS manages what actions users can perform with the key. Even if someone has the key, they still lack permission to encrypt or decrypt unless explicitly allowed.

Despite encrypting my Amazon DynamoDB table, I could still see the table's items because DynamoDB uses transparent data encryption.

This means that the data is automatically encrypted at rest, but AWS decrypts it when you access it through authorized API calls. In other words, encryption happens behind the scenes, so applications and users with proper permissions can read and write data normally, while unauthorized access is blocked.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-kms_c0d1e2f3)

---

## Denying Access

I configured a new IAM (Identity and Access Management) user to test encryption.

The permission policies I granted this user allow full access to DynamoDB, but no access to the KMS keys I created.

This setup ensures that the user can interact with the table normally but cannot decrypt or view the encrypted data, demonstrating how AWS enforces encryption and access control.

After accessing the DynamoDB table as the test user, I encountered an “Access Denied” error because this user does not have permission to use the KMS key that encrypts the table.

This confirmed that AWS encryption is working correctly: even though the user can access the table itself, they cannot view or decrypt the data without the proper key permissions.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-kms_w0x1y2z3)

---

## EXTRA: Granting Access

To allow my test user to use the encryption key, I added the test user to the key’s user section.

Using the test user, I tried to access the DynamoDB table and view the data, and I was able to see it instead of getting a “Refused” or “Denied” error.

Encryption secures the data itself, while access control protects the service and its resources. By combining both, access control acts as the first layer of security, and encryption adds a second layer to further protect the data.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-kms_feffb2fb8)

---

---
