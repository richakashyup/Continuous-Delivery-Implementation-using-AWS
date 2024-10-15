## [Continuous-Delivery-Implementation-using-AWS]

Overview

Created a continuous delivery pipeline for a simple web application.
First use a version control system (Git) to store source code. Then,
created a continuous delivery pipeline using AWS that will automatically
deploy web application whenever source code is updated.

What accomplish

create the continuous delivery pipeline discussed above. Learn to:

-   Set up a [[GitHub]](https://github.com/) repository for
    the application code

-   Create an [[AWS Elastic
    Beanstalk]](https://aws.amazon.com/elasticbeanstalk/?e=gs2020&p=cicd-intro) environment
    to deploy the application

-   Configure [[AWS
    CodeBuild]](https://aws.amazon.com/codebuild/?e=gs2020&p=cicd-intro) to
    build the source code from GitHub

-   Use [[AWS
    CodePipeline]](https://aws.amazon.com/codepipeline/?e=gs2020&p=cicd-intro) to
    set up the continuous delivery pipeline with source, build, and
    deploy stages

Prerequisites

We need to have :

-   An AWS account

-   A [[GitHub]](https://github.com/) account

-   [[Git]](https://git-scm.com/) installed on your computer

## Application architecture

  a visual representation of the services used in this tutorial and how they are connected. This application uses GitHub, AWS Elastic Beanstalk, AWS CodeBuild, and AWS CodePipeline.

![image](https://github.com/user-attachments/assets/0aa60d09-7474-4899-9b6d-db0aa4adfad8)


**[Exercise-1 : Setting up git repository]**

**[Fork the starter repo]**

1.  In a new browser tab, navigate
    to [[GitHub]](https://github.com/) and make sure you are
    logged into your account.

2.  Open the [aws-elastic-beanstalk-express-js-sample] repo.

3.  Choose the white Fork button on the top right corner of the screen.
    Next, create a fork page will appear. Create it

![image](https://github.com/user-attachments/assets/6a653f99-e930-4727-95a6-46650bba7f68)


4.  After a few seconds, your browser will display a copy of the repo in
    your account under Repositories.

![image](https://github.com/user-attachments/assets/175473c4-32ec-48e8-8019-3914e095a8cb)


**[Push a change to new repo]**

1.  Go to
    the [[repository]](https://github.com/aws-samples/aws-elastic-beanstalk-express-js-sample) and
    choose the green Code button near the top of the page.

2.  To clone the repository using HTTPS, confirm that the heading
    says *Clone with HTTPS.* If not, select the Use HTTPS link.

3.  Choose the white button with a clipboard icon on it (to the right of
    the URL).

![image](https://github.com/user-attachments/assets/5e61bc99-6ccd-40c8-9958-5167b99a1b99)



4\. For Windows, Open command prompt

5\. Run command

git clone
<https://github.com/YOUR-USERNAME/aws-elastic-beanstalk-express-js-sample>

replace YOUR-USERNAME with your github username

![image](https://github.com/user-attachments/assets/a3c52455-b40b-4a71-ada0-76f574be5e8f)


We didn't specify the location of the repo. So, it get clone at the
location
**C:\\Windows\\System32\\aws-elastic-beanstalk-express-js-sample**

6.  In the new folder there is a file named app.js. Open app.js in your
    favourite code editor.

![image](https://github.com/user-attachments/assets/2696eac6-3588-4ca3-a9a6-4b88ed33fe34)


7.  Change the message in line 5 to say something other than \"Hello
    World!\" and save the file.

![image](https://github.com/user-attachments/assets/82e22e09-5e8f-49c3-bb19-e262138dc8f5)


8\. Go to the folder created with the
name aws-elastic-beanstalk-express-js-sample/ and Commit the change with
the following commands:

git add app.js

git commit -m \"change message\"

![image](https://github.com/user-attachments/assets/b2ec3931-4f56-41e8-b475-86e053111ce7)


1.  Push the local changes to the remote repo hosted on GitHub with the
    following command.

git push

Note that you need to configure Personal access tokens
(classic) under Developer Settings in GitHub for remote authentication

![image](https://github.com/user-attachments/assets/db045dcc-47fc-4346-8038-a83645d8ce6a)


**[Check the change]**

1.  In your browser window,
    open [[GitHub]](https://github.com/).

2.  In the left navigation panel, under Repositories, select the one
    named aws-elastic-beanstalk-express-js-sample.

3.  Choose the app.js file. The contents of the file, including your
    change, should be displayed.

![image](https://github.com/user-attachments/assets/b792b14b-a5c2-4267-995e-3af9acd8bd02)


Exercise 2 Deploy Web App

Configure AWS Elastic Beanstalk App

1.  In a new browser tab, open the [[AWS Elastic Beanstalk
    console]](https://console.aws.amazon.com/elasticbeanstalk/home?region=us-west-2#/welcome).

2.  Choose the orange Create Application button.

![image](https://github.com/user-attachments/assets/a75a6527-0f6c-40ed-aacc-f95094450552)


3.  Choose Web server environment under the Configure environment
    heading.

4.  In the text box under the heading Application name,
    enter DevOpsGettingStarted*.*

![image](https://github.com/user-attachments/assets/774fe3a7-1b07-45bb-8228-72aa52f9a327)


![image](https://github.com/user-attachments/assets/c05e5f62-f2a2-4c88-b5cc-95298d35e57e)


5.  In the Platform dropdown menu, under the Platform heading,
    select Node.js . Platform branch and Platform version will
    automatically populate with default selections.

![image](https://github.com/user-attachments/assets/9b03c6df-8344-49c1-aa97-3a935d311f8c)


6.  Confirm that the radio button next to Sample application under the
    Application code heading is selected.

7.  Confirm that the radio button next to Single instance (free tier
    eligible) under the Presets heading is selected.

8.  Select Next.

![image](https://github.com/user-attachments/assets/466266b0-256e-4200-baf0-35366fe5a74f)


9\. Choose one of the following, based on the values displayed in your
list.

-   If *aws-elasticbeanstalk-ec2-role* displays in the dropdown list,
    select it from the EC2 instance profile dropdown list.

-   If another value displays in the list, and it's the default EC2
    instance profile intended for your environments, select it from the
    EC2 instance profile dropdown list.

-   If the EC2 instance profile dropdown list doesn\'t list any values
    to choose from, expand the procedure that follows, *Create IAM Role
    for EC2 instance profile*.

> Search for IAM and then click on Roles
>
![image](https://github.com/user-attachments/assets/4c755067-b99b-496b-9cac-840ccb51a353)

>
> Click on Create Role
>
![image](https://github.com/user-attachments/assets/a4ececc2-4d74-4fb7-b8a0-8843533e2894)

>
> Select AWS Services and then select CodeDeploy under use cases and
> then click on Next
>
![image](https://github.com/user-attachments/assets/f22778ac-cd33-49f1-b633-57792406bf50)

>
![image](https://github.com/user-attachments/assets/0ce76158-866f-4d31-b506-2d5f72c735a8)

>
> On the **Add permissions** page, the correct permissions policy for
> the use case is displayed. Choose **Next**.
>
![image](https://github.com/user-attachments/assets/ec23940a-8353-4863-930a-62545bfcb509)

>
> On the **Name, review, and create** page, in **Role name**, enter a
> name for the service role (for example, **CodeDeployServiceRole**),
> and then choose **Create role**.
>
![image](https://github.com/user-attachments/assets/c9f2415b-20fa-40c7-82a7-4868f2ee0ff7)

>
![image](https://github.com/user-attachments/assets/2704a059-67e3-463c-8e7e-07928d195f6a)

>![image](https://github.com/user-attachments/assets/6f75c8f0-cd4f-4369-811b-58119dbe1c71)

>
![image](https://github.com/user-attachments/assets/0b899dfa-59e1-47f3-8d95-762ced76eec1)


To grant the service role access to only some supported endpoints,

1.  Go to the role you have created and then Choose the **Trust
    relationships** tab. And then Choose **Edit trust policy**. And then
    update with the following code.
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "",
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    
                    "codedeploy.us-east-1.amazonaws.com",
                    "codedeploy.us-east-2.amazonaws.com",
                    "codedeploy.us-west-1.amazonaws.com",
                    "codedeploy.us-west-2.amazonaws.com",
                    "codedeploy.ca-central-1.amazonaws.com",
                    "codedeploy.ap-east-1.amazonaws.com",                  
                    "codedeploy.ap-northeast-1.amazonaws.com",
                    "codedeploy.ap-northeast-2.amazonaws.com",
                    "codedeploy.ap-northeast-3.amazonaws.com",
                    "codedeploy.ap-southeast-1.amazonaws.com",
                    "codedeploy.ap-southeast-2.amazonaws.com",
                    "codedeploy.ap-southeast-3.amazonaws.com",
                    "codedeploy.ap-southeast-4.amazonaws.com",
                    "codedeploy.ap-south-1.amazonaws.com",
                    "codedeploy.ap-south-2.amazonaws.com",
                    "codedeploy.ca-central-1.amazonaws.com",
                    "codedeploy.eu-west-1.amazonaws.com",
                    "codedeploy.eu-west-2.amazonaws.com",
                    "codedeploy.eu-west-3.amazonaws.com",
                    "codedeploy.eu-central-1.amazonaws.com",
                    "codedeploy.eu-central-2.amazonaws.com",
                    "codedeploy.eu-north-1.amazonaws.com",
                    "codedeploy.eu-south-1.amazonaws.com",
                    "codedeploy.eu-south-2.amazonaws.com",
                    "codedeploy.il-central-1.amazonaws.com",
                    "codedeploy.me-central-1.amazonaws.com",
                    "codedeploy.me-south-1.amazonaws.com",
                    "codedeploy.sa-east-1.amazonaws.com"
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}

```
![image](https://github.com/user-attachments/assets/3c13c3b2-7414-4861-b0e3-b615994ddbe2)

>
![image](https://github.com/user-attachments/assets/00f45b90-0d63-4601-aa0d-44ee11c3a5ab)


![image](https://github.com/user-attachments/assets/38e47e9d-ebf3-40de-800e-d9d621ac1e01)


and then choose **Update policy**.

![image](https://github.com/user-attachments/assets/f18ad7e1-0a3f-42a5-82c6-b8bd195e09e2)


## 

## Go to IAM identity centre console. Then click on enable. The you will reach to IAM identity centre Dashboard. 

![image](https://github.com/user-attachments/assets/029514a0-39db-4d1e-8e9c-e39c1178b47a)


In the navigation pane, choose **Permission sets**, and then
choose **Create permission set**.

![image](https://github.com/user-attachments/assets/ee7695c0-0dc7-472b-9806-8c9c21974b5a)




![image](https://github.com/user-attachments/assets/ff5aeea1-31be-414a-9067-a6bdcfc88391)


Choose **Custom permission set**.

Choose **Next**.

![image](https://github.com/user-attachments/assets/02c68c93-b645-4cd2-809e-b3cb7ab1833e)


Choose **Inline policy**.

![image](https://github.com/user-attachments/assets/926d9025-479b-43bd-97c0-ef9f305e616d)


Remove the sample code.

Add the following policy code:
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "CodeDeployAccessPolicy",
      "Effect": "Allow",
      "Action": [
        "autoscaling:*",
        "codedeploy:*",
        "ec2:*",
        "lambda:*",
        "ecs:*",
        "elasticloadbalancing:*",
        "iam:AddRoleToInstanceProfile",
        "iam:AttachRolePolicy",
        "iam:CreateInstanceProfile",
        "iam:CreateRole",
        "iam:DeleteInstanceProfile",
        "iam:DeleteRole",
        "iam:DeleteRolePolicy",
        "iam:GetInstanceProfile",
        "iam:GetRole",
        "iam:GetRolePolicy",
        "iam:ListInstanceProfilesForRole",
        "iam:ListRolePolicies",
        "iam:ListRoles",
        "iam:PutRolePolicy",
        "iam:RemoveRoleFromInstanceProfile",
        "s3:*",
        "ssm:*",
        "cloudformation:*"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CodeDeployRolePolicy",
      "Effect": "Allow",
      "Action": [
        "iam:PassRole"
      ],
      "Resource": "arn:aws:iam::account-ID:role/CodeDeployServiceRole"
    }
  ]
}

```
![image](https://github.com/user-attachments/assets/87819e7d-d2ab-4527-9c1f-6625e7410d6b)


In this policy,
replace *arn:aws:iam::account-ID:role/CodeDeployServiceRole* with the
ARN value of the CodeDeploy service role that you created

The preceding policy lets you deploy an application to an
EC2/On-Premises compute platform

Also use the AWS CloudFormation templates to launch Amazon EC2 instances
that are compatible with CodeDeploy.

Choose **Next**.

In **Permission set name**, enter:

**CodeDeployUserPermissionSet**

![image](https://github.com/user-attachments/assets/522c1cc7-7b08-4237-94d9-b33750cd9d79)


Choose **Next**.

On the **Review and create** page, review the information and
choose **Create**.

![image](https://github.com/user-attachments/assets/e63bc469-5983-49de-a7a4-d2bcbdca3011)


![image](https://github.com/user-attachments/assets/53f26830-d3a9-4d0b-9e67-d330d4c2d5bf)


![image](https://github.com/user-attachments/assets/b3fa14f5-7813-49cf-8228-fd07d58771b4)

###### To assign the permission set to the CodeDeploy administrative user

1.  In the navigation pane, choose **AWS accounts**, and then select the
    check box next to the AWS account that you\'re currently signed in
    to.

2.  Choose the **Assign users or groups** button.

![image](https://github.com/user-attachments/assets/e01d82fe-a612-46b4-b94f-f20568c9f3cc)

>
![image](https://github.com/user-attachments/assets/257c822b-5cec-455c-9ddc-d1cd266b22ac)


3.  Choose the **Users** tab.

4.  Select the check box next to the CodeDeploy administrative user.

5.  Choose **Next**.

![image](https://github.com/user-attachments/assets/ad721bfc-4ac9-4d34-9ebf-4ba1d992d21a)


6.  Select the check box next to CodeDeployUserPermissionSet.

7.  Choose **Next**.

![image](https://github.com/user-attachments/assets/bd44c5a9-b6b8-47ab-95c8-014ae77cd582)

8.  Review the information and choose **Submit**.

![image](https://github.com/user-attachments/assets/f06c56e1-2bdd-4ae6-899f-b7a114cca874)
![image](https://github.com/user-attachments/assets/9a7f6c36-30bd-438d-9fcf-28ca7949a467)

>
> You have now assigned the CodeDeploy administrative user
> and CodeDeployUserPermissionSet to your AWS account, binding them
> together.

###### To sign out and sign back in as the CodeDeploy administrative user

2.  Before you sign out, make sure you have the AWS access portal URL
    and the username and one-time password for the CodeDeploy
    adminstrative user.

3.  Sign out of AWS.

4.  Paste the AWS access portal URL into your browser\'s address bar.

5.  Sign in as the CodeDeploy adminstrative user.

6.  An **AWS account** box appears on the screen.

![image](https://github.com/user-attachments/assets/2ed31bcb-d87d-4f8b-af17-cae0b9293823)



7.  Choose **AWS account**, and then choose the name of the AWS account
    to which you assigned the CodeDeploy adminstrative user and
    permission set.

![image](https://github.com/user-attachments/assets/c1ad9005-80ad-4a1c-afcd-b09e25034bb5)


8.  Next to the CodeDeployUserPermissionSet,

> The AWS Management Console appears. You are now signed in as the
> CodeDeploy adminstrative user with the limited permissions. You can
> now perform CodeDeploy-related operations,
> and *only* CodeDeploy-related operations, as this user.
>
![image](https://github.com/user-attachments/assets/c04d239b-a23b-4098-bbaf-9a914b9bfd20)


1.  On your development machine, create a text file
    named CodeDeployDemo-EC2-Trust.json. Paste the following content,
    which allows Amazon EC2 to work on your behalf:
```
2.	{
3.	    "Version": "2012-10-17",
4.	    "Statement": [
5.	        {
6.	            "Sid": "",
7.	            "Effect": "Allow",
8.	            "Principal": {
9.	                "Service": "ec2.amazonaws.com"
10.	            },
11.	            "Action": "sts:AssumeRole"
12.	        }
13.	    ]
}

```
14. In the same directory, create a text file
    named CodeDeployDemo-EC2-Permissions.json. Paste the following
    content:
```
15.	{
16.	    "Version": "2012-10-17",
17.	    "Statement": [
18.	        {
19.	            "Action": [
20.	                "s3:Get*",
21.	                "s3:List*"
22.	            ],
23.	            "Effect": "Allow",
24.	            "Resource": "*"
25.	        }
26.	    ]
}

```
![image](https://github.com/user-attachments/assets/210f782f-cbc6-4815-96d9-ef8e957a05cf)
![image](https://github.com/user-attachments/assets/07fd4577-3e93-40db-92ef-fdc20a705685)

>
> From the same directory, call the **create-role** command to create an
> IAM role named **CodeDeployDemo-EC2-Instance-Profile**, based on the
> information in the first file:
>
> aws iam create-role \--role-name CodeDeployDemo-EC2-Instance-Profile
> \--assume-role-policy-document <file://CodeDeployDemo-EC2-Trust.json>
>
![image](https://github.com/user-attachments/assets/14f66379-f842-49fc-981e-00bded6c7b80)


From the same directory, call the **put-role-policy** command to give
the role named **CodeDeployDemo-EC2-Instance-Profile** the permissions
based on the information in the second file:

aws iam put-role-policy \--role-name CodeDeployDemo-EC2-Instance-Profile
\--policy-name CodeDeployDemo-EC2-Permissions \--policy-document
<file://CodeDeployDemo-EC2-Permissions.json>

![image](https://github.com/user-attachments/assets/d06a3ed0-8946-44cd-8536-0a20736c6563)


> Call the **attach-role-policy** to give the role Amazon EC2 Systems
> Manager permissions so that SSM can install the CodeDeploy agent. This
> policy is not needed if you plan to install the agent from the public
> Amazon S3 bucket with the command line. 
>
> aws iam attach-role-policy \--policy-arn
> arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore \--role-name
> CodeDeployDemo-EC2-Instance-Profile

![image](https://github.com/user-attachments/assets/00494900-28eb-48bd-b996-e058efdb0d43)


Call the **create-instance-profile** command followed by
the **add-role-to-instance-profile** command to create an IAM instance
profile named **CodeDeployDemo-EC2-Instance-Profile**. The instance
profile allows Amazon EC2 to pass the IAM role
named **CodeDeployDemo-EC2-Instance-Profile** to an Amazon EC2 instance
when the instance is first launched:

aws iam create-instance-profile \--instance-profile-name
CodeDeployDemo-EC2-Instance-Profile

![image](https://github.com/user-attachments/assets/59ff75e3-a29d-41a4-af41-e1a4dac9af32)


aws iam add-role-to-instance-profile \--instance-profile-name
CodeDeployDemo-EC2-Instance-Profile \--role-name
CodeDeployDemo-EC2-Instance-Profile

![image](https://github.com/user-attachments/assets/7e05c982-1237-4542-a594-0a020bcf77c3)


Note :- Before running above commands create a user and then click on
the user you created then add permission then create a inline policy
select json

1.
```
1.	Iam:CreateRole
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:CreateRole",
            "Resource": "*"
        }
    ]
}

```
2.
```
iam:PutRolePolicy
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:PutRolePolicy",
            "Resource": "arn:aws:iam::730335558949:role/CodeDeployDemo-EC2-Instance-Profile"
        }
    ]
}
```
3.
```
iam:AttachRolePolicy
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:AttachRolePolicy",
            "Resource": "arn:aws:iam::730335558949:role/CodeDeployDemo-EC2-Instance-Profile"
        }
    ]
}

```
4.
```
iam:CreateInstanceProfile
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:CreateInstanceProfile",
            "Resource": "*"
        }
    ]
}

```
5.
```
iam:AddRoleToInstanceProfile
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:AddRoleToInstanceProfile",
            "Resource": "arn:aws:iam::730335558949:instance-profile/CodeDeployDemo-EC2-Instance-Profile"
        }
    ]
}

```
6.
```
iam:PassRole
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "arn:aws:iam::730335558949:role/CodeDeployDemo-EC2-Instance-Profile"
        }
    ]
}

```
-   Now that you\'ve created an IAM Role, and refreshed the list, it
    displays as a choice in the dropdown list. Select the IAM Role you
    just created from the EC2 instance profile dropdown list.

<!-- -->

-   10\. Choose Skip to Review on the Configure service access page.

-       This will select the default values for this step and skip the
    optional steps.

![image](https://github.com/user-attachments/assets/da7ae970-c722-4038-a119-e681cf27b1ce)


11\. The Review page displays a summary of all your choices.

12\. Choose Submit at the bottom of the page to initialize the creation
of your new environment.

![image](https://github.com/user-attachments/assets/c89a7212-dd69-4c9f-8ebe-e17ac11ffcb6)


![image](https://github.com/user-attachments/assets/b90bf739-c2b1-45f4-a0ea-f7cc75196c68)


Environment will successfully launched

![image](https://github.com/user-attachments/assets/62362da5-d579-4bb6-8ae2-9b65b4d102d8)


Test your Web app

1.  To test your sample web app, select the link under the name of your
    environment.

![image](https://github.com/user-attachments/assets/3e95dfdf-c4e3-41c6-9b45-b828965f8386)


2\. Once the test has completed, a new browser tab should open with a
webpage congratulating you!

![image](https://github.com/user-attachments/assets/584e7676-2dc9-4f9d-ae3e-427c58907f0d)


**[Create Build Project]**

Configure the AWS CodeBuild project

1.  In a new browser tab, open the [AWS CodeBuild
    console](https://console.aws.amazon.com/codesuite/codebuild/start?region=us-west-2).

2.  Choose the orange Create project button.

![image](https://github.com/user-attachments/assets/d58e5d86-1d84-4402-9e80-ca75e9670f89)

>
![image](https://github.com/user-attachments/assets/0149f5a2-674b-4cec-87ca-a2e7d31768f9)


3.  In the Project name field, enter *Build-DevOpsGettingStarted.*

4.  Select GitHub from the Source provider dropdown menu.

![image](https://github.com/user-attachments/assets/b76b6a1d-c11b-421a-be90-e90386848f6a)


5.  Confirm that the Connect using OAuth radio button is selected.

6.  Choose the white Connect to GitHub button. A new browser tab will
    open asking you to give AWS CodeBuild access to your GitHub repo.

7.  Choose the green Authorize aws-codesuite button.

![image](https://github.com/user-attachments/assets/b6beab52-3709-4664-a7ce-d614f47760a4)

>
![image](https://github.com/user-attachments/assets/a20371ce-589c-40c3-8aa9-8462c68d0bfc)


8.  Enter your GitHub password.

![image](https://github.com/user-attachments/assets/f5785451-c6d4-4e4c-9ebb-4148edebd2fa)


9.  Choose the orange Confirm button.

![image](https://github.com/user-attachments/assets/7098864c-4f10-440a-a29c-f57e563e30ac)


10. Select Repository in my GitHub account.

11. Enter *aws-elastic-beanstalk-express-js-sample* in the search field.

12. Select the repo you forked in very first step. After selecting your
    repo, your screen should look like this:

![image](https://github.com/user-attachments/assets/7f19642e-a7bb-466c-8df0-63602d9c647f)


13. Confirm that Managed Image is selected.

![image](https://github.com/user-attachments/assets/bda06d4c-16c0-4e71-b155-ddfc74a42c33)


14\. Select Amazon Linux 2 from the Operating system dropdown menu.

15\. Select Standard from the Runtime(s) dropdown menu.

16\. Select aws/codebuild/amazonlinux2-x86_64-standard:3.0 from
the Image dropdown menu.

17\. Confirm that Always use the latest image for this runtime
version is selected for Image version.

![image](https://github.com/user-attachments/assets/48de3ec1-dfb0-40f4-8e23-21043c6b9d9d)


18\. Confirm that New service role is selected.

![image](https://github.com/user-attachments/assets/6429f27d-e5bb-4283-9a72-30bb2fd311aa)


Create a Buildspec file for the Project

1.  Select Insert build commands.

![image](https://github.com/user-attachments/assets/b726ade7-7e91-408c-a756-a874aeef56ee)


2.  Choose Switch to editor.

![image](https://github.com/user-attachments/assets/841a8d57-c32c-4737-8003-eec9ae96f994)


3.  Replace the Buildspec in the editor with the code below:
```
version: 0.2
phases:
    build:
        commands:
            - npm i --save
artifacts:
    files:
        - '**/*'

```
![image](https://github.com/user-attachments/assets/7d6e5b2c-6863-4db4-a583-af7b82bddcca)


4.  Choose the orange Create build project button. You should now see a
    dashboard for your project.

![image](https://github.com/user-attachments/assets/4eeefab5-1e4e-4ffa-9c5b-a2f49befb524)


5.  Successfully created

![image](https://github.com/user-attachments/assets/b1ce1ada-4e51-45ac-8023-ffde13e2da9f)


Test the CodeBuild Project

1.  Choose the orange Start build button. This will load a page to
    configure the build process.

![image](https://github.com/user-attachments/assets/3b2a5665-799a-48c5-9726-b6185317052d)


2.  Confirm that the loaded page references the correct GitHub repo.

3.  Wait for the build to complete. As you are waiting you should see a
    green bar at the top of the page with the message *Build
    started,* the progress for your build under Build log, and, after a
    couple minutes, a green checkmark and a *Succeeded* message
    confirming the build worked.

![image](https://github.com/user-attachments/assets/602a6198-634e-437b-85fa-c397ea769562)


**[Create Delivery Pipeline]**

Create a new Pipeline

1.  In a browser window, open the [AWS CodePipeline
    console](https://console.aws.amazon.com/codesuite/codepipeline/start?region=us-west-2).

2.  Choose the orange Create pipeline button. A new screen will open up
    so you can set up the pipeline.

![image](https://github.com/user-attachments/assets/f338cc36-f223-47e9-9c1f-3091c73a04c1)

>
![image](https://github.com/user-attachments/assets/3f89615d-4f92-4f65-abdc-2d7cdb304fb8)


3.  In the Pipeline name field, enter *Pipeline-DevOpsGettingStarted.*

![image](https://github.com/user-attachments/assets/a8b8f480-9689-431f-9254-5bda8f9d2e8b)


4.  Confirm that New service role is selected.

![image](https://github.com/user-attachments/assets/d87315f3-0f34-4221-aeef-975de05f9092)


5.  Choose the orange Next button.

Configure Source Stage

1.  Select GitHub version 1 from the Source provider dropdown menu.

![image](https://github.com/user-attachments/assets/ca1de8ac-7a17-4f85-8371-be1f436ff121)


2.  Choose the white Connect to GitHub button. A new browser tab will
    open asking you to give AWS CodePipeline access to your GitHub repo.

![image](https://github.com/user-attachments/assets/24b35e14-932b-4de4-b4c4-4cb893aeda41)

3.  Choose the green Authorize aws-codesuite button. Next, you will see
    a green box with the message *You have successfully configured the
    action with the provider.*

![image](https://github.com/user-attachments/assets/036ea76d-e51d-4df4-957f-4ae6265511f9)


4.  From the Repository dropdown, select the repo you created in Module
    1.

5.  Select main from the branch dropdown menu.

6.  Confirm that GitHub webhooks is selected.

7.  Choose the orange Next button.

![image](https://github.com/user-attachments/assets/67587d7f-cec4-43b8-ae50-293574a6e7c4)


Configure the Build Stage

1.  From the Build provider dropdown menu, select AWS CodeBuild.

![image](https://github.com/user-attachments/assets/ba515fa4-9eba-4866-8354-839117bf930a)


2.  Under Region select closest region to you.

![image](https://github.com/user-attachments/assets/a7257aa6-4e87-460b-95ef-2c7c19900bb5)


3.  Select Build-DevOpsGettingStarted under Project name.

![image](https://github.com/user-attachments/assets/e8a19877-9840-4149-a7b7-63bb218bea79)

4.  Choose the orange Next button.

Configure the deploy stage

1.  Select AWS Elastic Beanstalk from the Deploy provider dropdown menu.

2.  Select the region closest to you under region.

![image](https://github.com/user-attachments/assets/17f1868f-7941-4b9a-9a16-29a9b8bc3555)


3.  Select the field under Application name and confirm you can see the
    app DevOpsGettingStarted created in Module 2.

4.  Select DevOpsGettingStarted-env from the Environment name textbox.

![image](https://github.com/user-attachments/assets/d2ae2e9f-d0c6-4800-9fc3-c4b062cfdd00)


5.  Choose the orange Next button. You will now see a page where you can
    review the pipeline configuration.

6.  Choose the orange Create pipeline button.

7.  The pipeline will be created successfully

![image](https://github.com/user-attachments/assets/3fb3a694-c1d4-480b-b2b1-3f989d7a3cf9)


Watch first Pipeline Execution

1.  Once the Deploy stage has switched to green and it
    says *Succeeded,* choose AWS Elastic Beanstalk. A new tab listing
    your AWS Elastic Beanstalk environments will open.

![image](https://github.com/user-attachments/assets/0bd7a5a7-60f5-481d-bc0a-dce5a287db75)


2.  Select the URL in the Devopsgettingstarted-env row. You should see a
    webpage with a white background and the text you included in your
    GitHub commit in Module 1.

![image](https://github.com/user-attachments/assets/00422274-533f-4e45-8847-9bdae490056c)

![1](https://github.com/user-attachments/assets/f18d6506-b111-48bc-b9ca-f02f46340265)


**[Finalize Pipeline and Test]**

Create a review stage in pipeline

1.  Open the [AWS CodePipeline
    console](https://console.aws.amazon.com/codesuite/codepipeline/pipelines?region=us-west-2).

2.  You should see the pipeline we created, which was
    called Pipeline-DevOpsGettingStarted. Select this pipeline.

![image](https://github.com/user-attachments/assets/b8ba2dc9-75d4-4ee4-bcb0-1c19b8a6b138)


3.  Choose the white Edit button near the top of the page.

![image](https://github.com/user-attachments/assets/ec8ea8a5-288e-4799-92eb-58030959021c)


4.  Choose the white Add stage button between
    the Build and Deploy stages.

![image](https://github.com/user-attachments/assets/9221b5ec-b912-4791-89d6-ed465898fb89)


5.  In the Stage name field, enter *Review.*

6.  Choose the orange Add stage button.

![image](https://github.com/user-attachments/assets/2290a047-ea60-410a-ae29-ba671170a662)


7.  In the Review stage, choose the white Add action group button.

![image](https://github.com/user-attachments/assets/029f9544-4998-4282-9ecf-4ef94a3acee0)


8.  Under Action name, enter *Manual_Review.*

9.  From the Action provider dropdown, select Manual approval.

![image](https://github.com/user-attachments/assets/9b478a51-9353-4974-aa6a-0502394b95fe)


10. Confirm that the optional fields have been left blank.

11. Choose the orange Done button.

12. Choose the orange Save button at the top of the page.

![image](https://github.com/user-attachments/assets/dceb8c0e-d98d-430e-938f-28cb34393b8d)


13. Choose the orange Save button to confirm the changes. You will now
    see your pipeline with four stages: Source, Build,
    Review, and Deploy.

![image](https://github.com/user-attachments/assets/a510529e-16f6-4b11-8c0b-a8f781bf975c)


Push a New commit Repo

1.  In your favorite code editor, open the app.js file from Module 1.

2.  Change the message in Line 5.

![image](https://github.com/user-attachments/assets/f214e127-f457-4c9a-9136-a48b37ce836a)


3.  Save the file.

4.  Open your preferred Git client.

5.  Navigate to the folder created in .

6.  Commit the change with the following commands

> git add app.js
>
> git commit -m \"final pipeline test\"
>
![image](https://github.com/user-attachments/assets/b32ac36a-6050-406d-9a38-636040ffac76)


7.  Push the local changes to the remote repo hosted on GitHub with the
    following command:

git push

![image](https://github.com/user-attachments/assets/bd11ab1f-f123-4d4e-91b5-51bc642ae081)


Monitor the pipeline and manually approve the change

1.  Navigate to the [AWS CodePipeline
    console](https://console.aws.amazon.com/codesuite/codepipeline/pipelines?region=us-west-2).

2.  Select the pipeline named Pipeline-DevOpsGettingStarted. Click on
    Release the Change.You should see the Source and Build stages switch
    from blue to green.

![image](https://github.com/user-attachments/assets/d057f7b6-927d-4b32-b3e5-9f53b973c281)


3.  When the Review stage switches to blue, choose the
    white Review button.

![image](https://github.com/user-attachments/assets/8f6de087-fb03-46ef-b840-9aca192c7cd0)

>
> Note:- add a Manual approval policy to user
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "codepipeline:PutApprovalResult",
                "codepipeline:GetPipelineExecution"
            ],
            "Resource": "*"
        }
    ]
}

```
4.  Select an approval.

5.  Click on Submit

![image](https://github.com/user-attachments/assets/35b8131e-9762-4170-96b7-432537d40241)


6.  Wait for the Review and Deploy stages to switch to green.

![image](https://github.com/user-attachments/assets/94975af7-6e88-4301-a423-fb9fe8292c71)


7.  Select the AWS Elastic Beanstalk link in the Deploy stage. A new tab
    listing your Elastic Beanstalk environments will open.

![image](https://github.com/user-attachments/assets/bb0a0a6b-319b-45cc-9b2d-776d7726f61c)


8.  Select the URL in the Devopsgettingstarted-env row. You should see a
    webpage with a white background and the text you had in your most
    recent GitHub commit.

![image](https://github.com/user-attachments/assets/8690d50b-6f14-49b2-8a31-3d8a86b05143)

>
![image](https://github.com/user-attachments/assets/00c6154d-0b84-4a8b-9c14-2c2440b9f44f)


Congratulations! You have a fully functional continuous delivery
pipeline hosted on AWS.
