# Use AWS

## Setting up for the first time

The following steps will get you from nothing with AWS to having an admin **IAM user** and a **root user** within a **root account**.

1. Go to the **AWS Console** and create a **root user**
2. You now have a **root user** within your **root account** that can login via email/password. This user does not show up in IAM and should only be used for billing/account management
3. On the top navigation bar, click on your username and click on **My Security Credentials**
4. Set up **Multi-Factor Authentication** for this **root user** in the **root account**
5. Go to IAM and create an **IAM user** with:
   1. Console access enabled 
   2. Programmatic access disabled
   3. The **AdministratorAccess** policy
   4. The password set to auto-generate
   5. Checked checkbox that says to reset the password on first login
6. Take note of:
   1. The generated password
   2. The IAM user's username
   3. Your Account ID
7. Log out of the **root user**
8. Go to the **AWS Console** and sign in using **IAM user**
9. Enter your Account ID
10. Enter your **IAM user**'s username and auto-generated password
11. Set your actual password this time
12. Set up **Mutli-Factor Authentication** for this **IAM user** that is in the **root account**.
13. You now have an **admin IAM user** within your **root account**. This user will show up in IAM and should only be used for creating other users \(eg. for Terraform/other admins\)

## Creating a sibling account

The following steps assume that the intiial setup of a **root user** has been done and you are logged in as that **root user** in the **root account**.

1. Go to AWS Organizations
2. Create a new organization so that it is a child of the Root account and a sibling of your management account
3. Log out of your **root user**
4. Go to the **AWS Console** and sing in using the **Root user**
5. Enter the email of the sibling organization
6. Click on **Forgot Password**
7. Go to your email and click on the link to set the password of the **root user** of the sibling account
8. Follow the steps in the first-time setup to setup the **root user** and the **IAM user** for this sibling account
