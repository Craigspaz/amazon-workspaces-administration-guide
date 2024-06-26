# Launch a WorkSpace using Simple AD<a name="launch-workspace-simple-ad"></a>

WorkSpaces enables you to provision virtual, cloud\-based Microsoft Windows desktops for your users, known as *WorkSpaces*\.

WorkSpaces uses directories to store and manage information for your WorkSpaces and users\. For your directory, you can choose from Simple AD, AD Connector, or AWS Directory Service for Microsoft Active Directory, also known as AWS Managed Microsoft AD\. In addition, you can establish a trust relationship between your AWS Managed Microsoft AD directory and your on\-premises domain\.

In this tutorial, we launch a WorkSpace that uses Simple AD\. For tutorials that use the other options, see [Launch a virtual desktop using WorkSpaces](launch-workspaces-tutorials.md)\.

**Topics**
+ [Before you begin](#prereqs-simple-ad)
+ [Step 1: Create a Simple AD directory](#create-simple-ad)
+ [Step 2: Create a WorkSpace](#create-workspace-simple-ad)
+ [Step 3: Connect to the WorkSpace](#connect-workspace-simple-ad)
+ [Next steps](#next-steps-simple-ad)

## Before you begin<a name="prereqs-simple-ad"></a>
+ Simple AD is not available in every Region\. Verify the supported Regions and [ select a Region](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/getting-started.html#select-region) for your Simple AD directory\. For more information about the supported Regions for Simple AD, see [ Region Availability for AWS Directory Service](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/regions.html)\.
+ WorkSpaces is not available in every Region\. Verify the supported Regions and select a Region for your WorkSpaces\. For more information about the supported Regions, see [WorkSpaces Pricing by AWS Region](https://aws.amazon.com/workspaces/pricing/)\.
+ When you launch a WorkSpace, you must select a WorkSpace bundle\. A bundle is a combination of an operating system, and storage, compute, and software resources\. For more information, see [Amazon WorkSpaces Bundles](https://aws.amazon.com/workspaces/details/#Amazon_WorkSpaces_Bundles)\.
+ When you create a directory using AWS Directory Service or launch a WorkSpace, you must create or select a virtual private cloud configured with a public subnet and two private subnets\. For more information, see [Configure a VPC for WorkSpaces](amazon-workspaces-vpc.md)\.

## Step 1: Create a Simple AD directory<a name="create-simple-ad"></a>

Create a Simple AD directory\. AWS Directory Service creates two directory servers, one in each of the private subnets of your VPC\. Note that there are no users in the directory initially\. You will add a user in the next step when you create the WorkSpace\.

**Note**  
Simple AD is made available to you free of charge to use with WorkSpaces\. If there are no WorkSpaces being used with your Simple AD directory for 30 consecutive days, this directory will be automatically deregistered for use with Amazon WorkSpaces, and you will be charged for this directory as per the [AWS Directory Service pricing terms](http://aws.amazon.com/directoryservice/pricing/)\.  
To delete empty directories, see [Delete the directory for your WorkSpaces](delete-workspaces-directory.md)\. If you delete your Simple AD directory, you can always create a new one when you want to start using WorkSpaces again\.

**To create a Simple AD directory**

1. Open the WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Choose **Set up Directory**, **Simple AD**, and **Next**\.

1. Configure the directory as follows:

   1. For **Organization name**, enter a unique organization name for your directory \(for example, my\-example\-directory\)\. This name must be at least four characters in length, consist of only alphanumeric characters and hyphens \(\-\), and begin or end with a character other than a hyphen\.

   1. For **Directory DNS name**, enter the fully\-qualified name for the directory \(for example, example\.com\)\.
**Important**  
If you need to update your DNS server after launching your WorkSpaces, follow the procedure in [Update DNS servers for Amazon WorkSpaces](update-dns-server.md) to ensure that your WorkSpaces get properly updated\.

   1. For **NetBIOS name**, enter a short name for the directory \(for example, example\)\.

   1. For **Admin password** and **Confirm password**, enter a password for the directory administrator account\. For more information about the password requirements, see [How to Create a Microsoft AD Directory](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/create_managed_ad.html) in the *AWS Directory Service Administration Guide*\.

   1. \(Optional\) For **Description**, enter a description for the directory\.

   1. For **Directory size**, choose **Small**\.

   1. For **VPC**, select the VPC that you created\.

   1. For **Subnets**, select the two private subnets \(with the CIDR blocks `10.0.1.0/24` and `10.0.2.0/24`\)\.

   1. Choose **Next**\.

1. Choose **Create directory**\.

1. The initial status of the directory is `Requested` and then `Creating`\. When directory creation is complete \(this might take a few minutes\), the status is `Active`\.

**What happens during directory creation**

WorkSpaces completes the following tasks on your behalf:
+ Creates an IAM role to allow the WorkSpaces service to create elastic network interfaces and list your WorkSpaces directories\. This role has the name `workspaces_DefaultRole`\.
+ Sets up a Simple AD directory in the VPC that is used to store user and WorkSpace information\. The directory has an administrator account with the user name Administrator and the specified password\.
+ Creates two security groups, one for directory controllers and another for WorkSpaces in the directory\.

## Step 2: Create a WorkSpace<a name="create-workspace-simple-ad"></a>

Now you are ready to launch the WorkSpace\.

**To create a WorkSpace for a user**

1. Open the WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Choose **Launch WorkSpaces**\.

1. On the **Select a Directory** page, do the following:

   1. For **Directory**, choose the directory that you created\.

   1. For **Enable Self Service Permissions**, choose **Yes** or **No** and enter a description\.

   1. For **Enable Amazon WorkDocs**, choose **Yes**\.
**Note**  
This option is available only if Amazon WorkDocs is available in the selected Region\.

   1. Choose **Next Step**\. WorkSpaces registers your Simple AD directory\.

1. On the **Identify Users** page, add a new user to your directory as follows:

   1. Complete **Username**, **First Name**, **Last Name**, and **Email**\. Use an email address that you have access to\.

   1. Choose **Create Users**\.

   1. Choose **Next Step**\.

1. On the **Select Bundle** page, select a bundle and then choose **Next Step**\.

1. On the **WorkSpaces Configuration** page, choose a running mode and then choose **Next Step**\.

1. On the **Review & Launch WorkSpaces** page, choose **Launch WorkSpaces**\. The initial status of the WorkSpace is `PENDING`\. When the launch is complete \(this can take up to 20 minutes\), the status is `AVAILABLE` and an invitation is sent to the email address that you specified for the user\.
**Note**  
Invitation emails aren't sent if the user already exists in Active Directory\. Instead, make sure you manually send the user an invitation email\. For more information, see [Send an invitation email](manage-workspaces-users.md#send-invitation)\.

## Step 3: Connect to the WorkSpace<a name="connect-workspace-simple-ad"></a>

After you receive the invitation email, you can connect to your WorkSpace using the client of your choice\. After you sign in, the client displays the WorkSpace desktop\.

**To connect to the WorkSpace**

1. Open the link in the invitation email\. When prompted, enter a password and activate the user\. Remember this password as you will need it to sign in to your WorkSpace\.
**Note**  
Passwords are case\-sensitive and must be between 8 and 64 characters in length, inclusive\. Passwords must contain at least one character from each of the following categories: lowercase letters \(a\-z\), uppercase letters \(A\-Z\), numbers \(0\-9\), and \~\!@\#$%^&\*\_\-\+=`\|\\\(\)\{\}\[\]:;"'<>,\.?/\.

1. Review [WorkSpaces Clients](https://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-clients.html) in the *Amazon WorkSpaces User Guide* for more information about the requirements for each client, and then do one of the following: 
   + When prompted, download one of the client applications or launch **Web Access**\.
   + If you aren't prompted and you haven't installed a client application already, open [https://clients\.amazonworkspaces\.com/](https://clients.amazonworkspaces.com/) and download one of the client applications or launch **Web Access**\.
**Note**  
You cannot use a web browser \(Web Access\) to connect to Amazon Linux WorkSpaces\.

1. Start the client, enter the registration code from the invitation email, and choose **Register**\.

1. When prompted to sign in, enter the user name and password for the user, and then choose **Sign In**\.

1. \(Optional\) When prompted to save your credentials, choose **Yes**\.

## Next steps<a name="next-steps-simple-ad"></a>

You can continue to customize the WorkSpace that you just created\. For example, you can install software and then create a custom bundle from your WorkSpace\. You can also perform various administrative tasks for your WorkSpaces and your WorkSpaces directory\. If you are finished with your WorkSpace, you can delete it\. For more information, see the following documentation\.
+ [Create a custom WorkSpaces image and bundle](create-custom-bundle.md)
+ [Administer your WorkSpaces](administer-workspaces.md)
+ [Manage directories for WorkSpaces](manage-workspaces-directory.md)
+ [Delete a WorkSpace](delete-workspaces.md)

For more information about using the WorkSpaces client applications, such as setting up multiple monitors or using peripheral devices, see [WorkSpaces Clients](https://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-clients.html) and [Peripheral Device Support](https://docs.aws.amazon.com/workspaces/latest/userguide/peripheral_devices.html) in the *Amazon WorkSpaces User Guide*\.