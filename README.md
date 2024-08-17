## 💻 STEP #1: Launch EC2 Instances with Tags

### 💡 What is EC2?
Amazon EC2 (Elastic Compute Cloud) is a service that lets you rent and use virtual computers in the cloud. These virtual computers, known as instances, can be customized and used for various tasks like running applications or hosting websites.

### Steps:
1. Log in to your AWS Management Console.
2. Head to EC2.
3. Switch your Region to the one closest to you.
4. In the EC2 console, choose **Launch instances**.

### 💡 What are EC2 instances?
If EC2 is the service that provides virtual computers/servers, each instance is one of those computers/servers that gets produced. You can customize your EC2 instance's CPU, memory, storage, networking capacity, and more.

5. In the **Name** field, enter the value `nextwork-production-yourname`. Replace `yourname` with your actual name.
6. Choose **Add additional tags** next to the Name field.
7. Add a new tag:
   - **Key:** `Env`
   - **Value:** `production`

### 💡 Why create a new tag?
Tags are labels attached to AWS resources for organization. In this case, the tag "Env" with the value "production" helps identify instances used in production vs. development environments.

8. Ensure the Amazon Machine Image (AMI) is using a **Free tier eligible** option.
9. Select an instance type that is **Free tier eligible** to avoid charges.
10. For **Key pair (login)**, select **Proceed without a key pair** (Not recommended for larger projects).
11. Click **Launch instance**.

### 💡 Note:
- We skipped configuring network and storage settings for simplicity. These settings are crucial for fine-tuning your EC2 instances' performance, security, and connectivity.

12. Repeat the steps to create another EC2 instance with the following tags:
   - **Name:** `nextwork-development-yourname`
   - **Env:** `development`
13. Take a screenshot of the Name and tags panel in this setup page.

---

## 📏 STEP #2: Create an IAM Policy

### Steps:
1. Head to your IAM console.
2. In the left-hand navigation panel, choose **Policies**.
3. Choose **Create policy**.
4. Switch your Policy editor tab to **JSON**.
5. Paste the following policy into your editor, replacing all existing code:

```json
{
 "Version": "2012-10-17",
 "Statement": [
   {
     "Effect": "Allow",
     "Action": "ec2:*",
     "Resource": "*",
     "Condition": {
       "StringEquals": {
         "ec2:ResourceTag/Env": "development"
       }
     }
   },
   {
     "Effect": "Allow",
     "Action": "ec2:Describe*",
     "Resource": "*"
   },
   {
     "Effect": "Deny",
     "Action": [
       "ec2:DeleteTags",
       "ec2:CreateTags"
     ],
     "Resource": "*"
   }
 ]
}
