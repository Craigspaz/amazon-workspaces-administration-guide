# Launch a WorkSpace using AWS Managed Microsoft AD<a name="launch-workspace-microsoft-ad"></a>

WorkSpaces enables you to provision virtual, cloud\-based Windows desktops for your users, known as *WorkSpaces*\.

WorkSpaces uses directories to store and manage information for your WorkSpaces and users\. For your directory, you can choose from Simple AD, AD Connector, or AWS Directory Service for Microsoft Active Directory, also known as AWS Managed Microsoft AD\. In addition, you can establish a trust relationship between your AWS Managed Microsoft AD directory and your on\-premises domain\.

In this tutorial, we launch a WorkSpace that uses AWS Managed Microsoft AD\. For tutorials that use the other options, see [Launch a virtual desktop using WorkSpaces](launch-workspaces-tutorials.md)\.

**Topics**
+ [Before you begin](#prereqs-microsoft-ad)
+ [Step 1: Create an AWS Managed Microsoft AD Directory](#create-microsoft-ad)
+ [Step 2: Create a WorkSpace](#create-workspace-microsoft-ad)
+ [Step 3: Connect to the WorkSpace](#connect-workspace-microsoft-ad)
+ [Next steps](#next-steps-microsoft-ad)

## Before you begin<a name="prereqs-microsoft-ad"></a>
+ WorkSpaces is not available in every Region\. Verify the supported Regions and select a Region for your WorkSpaces\. For more information about the supported Regions, see [WorkSpaces Pricing by AWS Region](https://aws.amazon.com/workspaces/pricing/)\.
+ When you launch a WorkSpace, you must select a WorkSpace bundle\. A bundle is a combination of an operating system, and storage, compute, and software resources\. For more information, see [Amazon WorkSpaces Bundles](https://aws.amazon.com/workspaces/details/#Amazon_WorkSpaces_Bundles)\.
+ When you create a directory using AWS Directory Service or launch a WorkSpace, you must create or select a virtual private cloud configured with a public subnet and two private subnets\. For more information, see [Configure a VPC for WorkSpaces](amazon-workspaces-vpc.md)\.

## Step 1: Create an AWS Managed Microsoft AD Directory<a name="create-microsoft-ad"></a>

First, create an AWS Managed Microsoft AD directory\. AWS Directory Service creates two directory servers, one in each of the private subnets of your VPC\. Note that there are no users in the directory initially\. You will add a user in the next step when you launch the WorkSpace\.

**Note**  
Shared directories are not currently supported for use with Amazon WorkSpaces\.
If your AWS Managed Microsoft AD directory has been configured for multi\-Region replication, only the directory in the primary Region can be registered for use with Amazon WorkSpaces\. Attempts to register the directory in a replicated Region for use with Amazon WorkSpaces will fail\. Multi\-Region replication with AWS Managed Microsoft AD isn't supported for use with Amazon WorkSpaces within replicated Regions\.

**To create an AWS Managed Microsoft AD directory**

1. Open the WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Choose **Set up Directory**, **Create Microsoft AD**\.

1. Configure the directory as follows:

   1. For **Organization name**, enter a unique organization name for your directory \(for example, my\-demo\-directory\)\. This name must be at least four characters in length, consist of only alphanumeric characters and hyphens \(\-\), and begin or end with a character other than a hyphen\.

   1. For **Directory DNS**, enter the fully\-qualified name for the directory \(for example, workspaces\.demo\.com\)\.
**Important**  
If you need to update your DNS server after launching your WorkSpaces, follow the procedure in [Update DNS servers for Amazon WorkSpaces](update-dns-server.md) to ensure that your WorkSpaces get properly updated\.

   1. For **NetBIOS name**, enter a short name for the directory \(for example, workspaces\)\.

   1. For **Admin password** and **Confirm password**, enter a password for the directory administrator account\. For more information about the password requirements, see [Create Your AWS Managed Microsoft AD Directory](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/create_managed_ad.html) in the *AWS Directory Service Administration Guide*\.

   1. \(Optional\) For **Description**, enter a description for the directory\.

   1. For **VPC**, select the VPC that you created\.

   1. For **Subnets**, select the two private subnets \(with the CIDR blocks `10.0.1.0/24` and `10.0.2.0/24`\)\.

   1. Choose **Next Step**\.

1. Choose **Create Microsoft AD**\.

1. Choose **Done**\. The initial status of the directory is `Creating`\. When directory creation is complete, the status is `Active`\.

## Step 2: Create a WorkSpace<a name="create-workspace-microsoft-ad"></a>

Now that you have created an AWS Managed Microsoft AD directory, you are ready to create a WorkSpace\.

**To create a WorkSpace**

1. Open the WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Choose **Launch WorkSpaces**\.

1. On the **Select a Directory** page, choose the directory that you created, and then choose **Next Step**\. WorkSpaces registers your directory\.

1. On the **Identify Users** page, add a new user to your directory as follows:

   1. Complete **Username**, **First Name**, **Last Name**, and **Email**\. Use an email address that you have access to\.

   1. Choose **Create Users**\.

   1. Choose **Next Step**\.

1. On the **Select Bundle** page, select a bundle and then choose **Next Step**\.

1. On the **WorkSpaces Configuration** page, choose a running mode and then choose **Next Step**\.

1. On the **Review & Launch WorkSpaces** page, choose **Launch WorkSpaces**\. The initial status of the WorkSpace is `PENDING`\. When the launch is complete, the status is `AVAILABLE` and an invitation is sent to the email address that you specified for the user\.
**Note**  
Invitation emails aren't sent if the user already exists in Active Directory\. Instead, make sure you manually send the user an invitation email\. For more information, see [Send an invitation email](manage-workspaces-users.md#send-invitation)\.

1. \(Optional\) If Amazon WorkDocs is supported in the Region, you can enable Amazon WorkDocs for all users in the directory\. For more information, see [Enable Amazon WorkDocs for AWS Managed Microsoft AD](enable-workdocs-active-directory.md)\. For more information about Amazon WorkDocs, see [ Amazon WorkDocs Drive](https://docs.aws.amazon.com/workdocs/latest/userguide/workdocs_drive_help.html) in the *Amazon WorkDocs Administration Guide*\.

## Step 3: Connect to the WorkSpace<a name="connect-workspace-microsoft-ad"></a>

After you receive the invitation email, you can connect to your WorkSpace using the client of your choice\. After you sign in, the client displays the WorkSpace desktop\.

**To connect to the WorkSpace**

1. Open the link in the invitation email\. When prompted, specify a password and activate the user\. Remember this password as you will need it to sign in to your WorkSpace\.
**Note**  
Passwords are case\-sensitive and must be between 8 and 64 characters in length, inclusive\. Passwords must contain at least one character from each of the following categories: lowercase letters \(a\-z\), uppercase letters \(A\-Z\), numbers \(0\-9\), and \~\!@\#$%^&\*\_\-\+=`\|\\\(\)\{\}\[\]:;"'<>,\.?/\.

1. Review [WorkSpaces Clients](https://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-clients.html) in the *Amazon WorkSpaces User Guide* for more information about the requirements for each client, and then do one of the following: 
   + When prompted, download one of the client applications or launch Web Access\.
   + If you aren't prompted and you haven't installed a client application already, open [https://clients\.amazonworkspaces\.com/](https://clients.amazonworkspaces.com/) and download one of the client applications or launch Web Access\.
**Note**  
You cannot use a web browser \(Web Access\) to connect to Amazon Linux WorkSpaces\.

1. Start the client, enter the registration code from the invitation email, and choose **Register**\.

1. When prompted to sign in, enter the user name and password for the user, and then choose **Sign In**\.

1. \(Optional\) When prompted to save your credentials, choose **Yes**\.

## Next steps<a name="next-steps-microsoft-ad"></a>

You can continue to customize the WorkSpace that you just created\. For example, you can install software and then create a custom bundle from your WorkSpace\. You can also perform various administrative tasks for your WorkSpaces and your WorkSpaces directory\. If you are finished with your WorkSpace, you can delete it\. For more information, see the following documentation\.
+ [Create a custom WorkSpaces image and bundle](create-custom-bundle.md)
+ [Administer your WorkSpaces](administer-workspaces.md)
+ [Manage directories for WorkSpaces](manage-workspaces-directory.md)
+ [Delete a WorkSpace](delete-workspaces.md)

For more information about using the WorkSpaces client applications, such as setting up multiple monitors or using peripheral devices, see [WorkSpaces Clients](https://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-clients.html) and [Peripheral Device Support](https://docs.aws.amazon.com/workspaces/latest/userguide/peripheral_devices.html) in the *Amazon WorkSpaces User Guide*\.