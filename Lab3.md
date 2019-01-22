# Lab 3: Web Apps and cloud services
  
## Exercise 1: Creating and configuring a WordPress web app
  
#### Task 1: Create a web app
  
1. Bbrowse to [**http://portal.azure.com**](http://portal.azure.com), and, if prompted, sign in by using the Microsoft account that is the Service Administrator of your Azure subscription.

1. In the hub menu, on the left side of the portal page, click **+ Create a resource**.

1. On the **New** blade, in the **Search the Marketplace** box, type **WordPress**, and then press Enter.

1. On the **Everything** blade, in the list of results, click **WordPress** from PUBLISHER **WordPress**.

1. On the **WordPress** blade, click **Create**.

1. On the **WordPress** blade, specify the following settings:

    - App name: type in a unique name that can consist of letters, digits, and hyphens

    - Subscription: ensure that the name of the target Azure subscription appears in the drop-down list

    - Resource Group: ensure that the **Create new** option is selected and, in the text box below, type **10979F0401-LabRG**

    - Database Provider: ensure that **Azure Database for MySQL** appears in the drop-down list

1. Click **App Service plan/Location**.

1. On the **App Service plan** blade, click **Create New**.

1. On the **New App Service plan** blade, in the **App Service plan** text box, type **10979F0401-sp**.

1. In the **Location** drop-down list, select the name of the Azure region that is available in your subscription and which is closest to the lab location.

1. Click **Pricing tier**.

1. Click the **Production** tab, click **S1** and click **Apply**.

1. On the **New App Service Plan** blade, click **OK**.

1. Back on the **WordPress** blade, click **Database Settings Required**.

1. On the **Database Server** blade, specify the following settings and click **OK**.

    - Server name: accept the default value

    - Server admin login name: **student**

    - Password: **Pa55w.rd1234**

    - Confirm password: **Pa55w.rd1234**

    - Version: **5.7**

    - Pricing tier: **Basic, 2 vCore(s), 5 GB**

    - Database name: accept the default value

1. Ensure that **Application Insights** setting is set to **Disabled**.

1. Click **Create**.

1. Wait until the provisioning completes.

    > **Note**: The provisioning might take a couple of minutes.


#### Task 2: Install WordPress
  
1. On MIA-CL1, in the Microsoft Edge window displaying the Azure portal, in the hub menu, click **Resource groups**.

1. On the **Resource groups** blade, click **10979F0401-LabRG**.

1. On the **10979F0401-LabRG** blade, click the entry representing the newly provisioned WordPress web app.

1. On the WordPress web app blade, click the link underneath the **URL** label. This will automatically open a new tab in Microsoft Edge displaying the new website.

1. On the WordPress website, in the languages list, click **English (United States)**, and then click **Continue**. 

1. On the Welcome page, complete the **Information needed** section with the following values:

    - Site Title: **Adatum Blog ######** (where **######** is a unique number)

    - Username: type the email address of the Microsoft account that you are using for this lab

    - Password: **Pa55w.rd0401**

    - Your E-mail: type the email address of the Microsoft account that you are using for this lab

    - Search Engine Visibility: accept the default setting

1. Click **Install WordPress**. 

1. Wait for the installation to complete.

    > **Note**: This might take a couple of minutes. If your connection to the web app times out, refresh the Microsoft Edge window.


#### Task 3: Create a blog post
  
1. In Microsoft Edge, on the WordPress web app webpage, click **Log in** and in the **Username or Email** box, type the email address of the Microsoft account that you are using for this lab.

1. In the **Password** box, type **Pa55w.rd0401**.

1. Select the **Remember Me** check box, and then click **Log In**.

    > **Note**: If prompted by Microsoft Edge to store the password for the website, click **No**.

1. On the **Dashboard** page, in the **Welcome to WordPress** section, click **Write your first blog post**.

1. If a **Welome to the wonderful world of blocks!** tip displays, close it and type **Welcome to the Adatum blog** in the **Add title** section.

1. In the **Start writing or type / to choose a block** area, type **Welcome to the Adatum blog**.

1. Click **Publish** to review the publish settings.  Click **Publish** again to finish.

1. Click the **View Post** link at the top of the page. Your new post appears. 

1. Close the Microsoft Edge tab displaying the WordPress interface, and then return to the Azure portal tab.

1. Close Microsoft Edge.


#### Task 4: Prepare for the next module

1. From MIA-CL1, start Microsoft Edge, browse to [**http://portal.azure.com**](http://portal.azure.com), and, if prompted, sign in by using the Microsoft account that is the Service Administrator of your Azure subscription.

1. In the Azure Portal, in the hub menu, click **Resource groups**.

1. On the **Resource groups** blade, click the ellipsis (**...**) on the right hand side of the **10979F0401-LabRG** entry.

1. Click **Delete**.

1. On the **Are you sure you want to delete "10979F0401-LabRG"?** blade, in the **TYPE THE RESOURCE GROUP NAME** text box, type **10979F0401-LabRG** and click **Delete**.

1. Close Microsoft Edge.

When you are finished with the lab, do not revert the virtual machines. Please keep all of the VMs running. The VMs in their current state are required for the next module.

> **Result**: After completing this exercise, you should have successfully created and configured an Azure web app to support WordPress blogs. 

