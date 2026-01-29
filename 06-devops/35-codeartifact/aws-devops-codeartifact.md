<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Secure Packages with CodeArtifact

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-codeartifact-updated)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

---

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codeartifact-updated_1d79e699)

---

## Introducing Today's Project!

In this project, I will :

🗂️ Set up CodeArtifact as a repository for the project's dependencies.
🛡️ Use IAM roles and policies to give the web app access to CodeArtifact.
✅ Verify the web app's connection to CodeArtifact!
💎Create and add my own packages to the CodeArtifact repository!

### Key tools and concepts

I learned how to use AWS CodeArtifact as a secure central repository for storing software packages, instead of downloading them directly from public sources like Maven Central. I also worked with IAM policies and roles to control permissions - policies are the actual rules that define what actions are allowed, while roles are containers that hold these policies and can be assigned to services like EC2 instances. I learned about Maven as a package manager for Java projects, and how to use authentication tokens for secure access to repositories. The project taught me the principle of least privilege, which means giving services only the minimum permissions they need to function safely.

### Project reflection

This project took me approximately 60 min

This project is part three of a series of DevOps projects where I'm building a CI/CD pipeline! I'll be working on the next project next day

---

## CodeArtifact Repository

CodeArtifact is a secure, central place to store all your software packages. 

A CodeArtifact domain is a container that groups multiple repositories belonging to the same project or organization. Domains are valuable because they provide centralized management—you can configure permissions and security settings once at the domain level, and they automatically apply to all repositories within it .

A CodeArtifact repository can have an upstream repository, which acts as a fallback source. When the primary repository doesn't contain a requested package, it automatically searches the upstream repository to fulfill the request.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codeartifact-updated_n4o5p6q7)

---

## CodeArtifact Security

### Issue

To access CodeArtifact, we need an authorization token that Maven will use to authenticate with the repository. This token acts like a temporary password that proves our EC2 instance has permission to download packages from CodeArtifact.
I ran into an error when retrieving the token because the EC2 instance doesn't have AWS credentials configured. The AWS CLI needs credentials (access keys or an IAM role) to make API calls to CodeArtifact. Since I haven't set up any credentials yet, the get-authorization-token command fails with "Unable to locate credentials."

### Resolution

To resolve the security token error, I granted the EC2 instance the necessary IAM permissions to access CodeArtifact. This resolved the error because the instance now has authorized access to authenticate and retrieve packages from the CodeArtifact repository

It is a security best practice to use IAM roles because they grant only the necessary permissions to a service, user, or application. This follows the principle of least privilege and is much safer than giving full access through access keys.

---

## The JSON policy attached to my role

This IAM policy grants four essential permissions for accessing CodeArtifact. First, codeartifact:GetAuthorizationToken allows the EC2 instance to request a temporary authentication token, which Maven uses as a password to connect to the repository. Second, codeartifact:GetRepositoryEndpoint enables retrieval of the repository's URL so Maven knows where to connect. Third, codeartifact:ReadFromRepository permits downloading packages from the repository—this is the core permission that allows Maven to fetch dependencies. Finally, sts:GetServiceBearerToken allows CodeArtifact to obtain validation tokens from AWS Security Token Service, with the condition restricting this permission exclusively to CodeArtifact for security purposes. The "Resource": "*" setting means these permissions apply to all CodeArtifact resources in your account.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codeartifact-updated_23rp7q8r9)

---

## Maven and CodeArtifact

### To test the connection between Maven and CodeArtifact, I compiled my web app using settings.xml

The settings.xml file enables Maven to work with CodeArtifact by storing the repository URL and authentication details. This tells Maven where to find dependencies and how to securely access the CodeArtifact repository. Once it’s configured, Maven can automatically connect, authenticate, and download required libraries without manual login each time.

Compiling means converting your project’s source code into a format that the computer can understand and execute. It’s like translating human-readable code into machine-ready instructions.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codeartifact-updated_c17eace8)

---

## Verify Connection

After compiling my web app, I checked my CodeArtifact Maven repository. I saw that my project’s dependencies had been downloaded from Maven Central and stored there, confirming that CodeArtifact was working correctly.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codeartifact-updated_1d79e699)

---

## Uploading My Own Packages

In a project extension, I also decided to:
- Create a custom package
- Publish it directly to my CodeArtifact repository
- Experience the full package lifecycle by downloading and using my own package

o create my own package, I opened CloudShell, created a test file (secret-mission.txt), and bundled it into a tar.gz package (secret-mission.tar.gz). I then generated a security hash using SHA256 to ensure the package’s integrity—this hash acts like a unique digital fingerprint, allowing CodeArtifact to verify the file wasn’t altered during upload.

fter publishing, CodeArtifact shows my secret-mission package in the repository’s package list. On the package details page, I can see:

- When it was published – the timestamp confirms it was just uploaded

- Version information – the version number I assigned to the package

- Origin – this repository is the source since it’s an original package

- Security hashes – the SHA256 fingerprint I generated, confirming the package’s integrity

This page essentially confirms that my package is successfully uploaded, secure, and ready to be used.

I validated my package by downloading it back from CodeArtifact using the aws codeartifact get-package-version-asset command. After downloading secret-mission.tar.gz, I extracted its contents with tar -xzvf and checked the file secret-mission.txt with cat. Seeing my original secret message confirmed that the package was successfully published, stored, and retrieved intact—completing the full package lifecycle.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codeartifact-updated_sm12-upload)

---

---
