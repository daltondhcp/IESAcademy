# Deploying Resources with Azure Resource Manager

# Lab Answer Key: Getting Started with Azure Resource Manager

## Exercise 1: Create Resource Groups

#### Task 1: Open the Azure Portal

1.  On the Taskbar, click the **Microsoft Edge** icon.

2.  In the open browser window, navigate to the **Azure Portal** (<https://portal.azure.com>).

3.  Enter the **email address** of your *Microsoft account*.

4.  Click the **Next** button.

5.  Enter the **password** for your *Microsoft account*.

6.  Click the **Sign in** button.

    > **Note**: If this is your first time logging in to the **Azure Portal**, you will see a dialog with a *tour* of the portal. Click **Get Started** to begin using the portal.

#### Task 2: Create a Resource Group using the Portal

1.  On the left side of the portal, click the **Create a resource** link.

    > The **Create a resource** icon is a *plus-sign* character.

2.  At the top of the **New** blade, locate the **Search the Marketplace** field.

3.  Enter the text **Resource Group** into the search field and press **Enter**.

4.  In the **Everything** search results blade, select the **Resource group** result.

5.  In the **Resource group** blade, click the **Create** button.

6.  In the additional **Resource group** blade, perform the following actions:

    a.  In the **Resource group name** field, enter the value **MOD02STOR**.

    b.  Leave the **Subscription** field set to its default value.

    c.  In the **Resource group location** field, select the **East US** location.

    d.  Click the **Create** button.

7.  Wait for the creation task to complete before moving on with this lab.

#### Task 3: Create a Resource Group using the Cloud Shell and Azure CLI

1.  At the top of the portal, click the **Cloud Shell** icon to open a new shell instance.

    > **Note**: The **Cloud Shell** icon is a symbol that is constructed of the combination of the *greater than* and *underscore* characters.

2.  Because this is your first time opening the **Cloud Shell** using your *Azure Pass* subscription, you will see a wizard to configure **Cloud Shell** for first-time usage. Perform the following actions:

    a.  When offered a choice between **Bash** or **PowerShell**, select the **Bash** option.

    b.  You will then see a dialog prompting you to create a new **Storage Account** to begin using the shell. You can safely accept the default settings and click the **Create storage** button.

    c.  Wait for the **Cloud Shell** to finish its first-time setup procedures before moving on with the lab.

    > If you do not see the configuration options for **Cloud Shell**, this is most likely because you are using an existing subscription with this course's labs. The labs are written from the perspective that you are using a new Azure Pass subscription. You may see some small discrepancies in future lab instructions.

3.  In the **Cloud Shell** command prompt at the bottom of the portal, type in the following command and press **Enter** to view a list of possible CLI commands:

    ```azurecli-interactive
        az --help
    ```

4.  Type in the following command and press **Enter** to view a list of possible CLI commands for *Resource Groups*:

    ```azurecli-interactive
        az group --help
    ```

5.  Type in the following command and press **Enter** to view a list of possible CLI commands to *create a Resource Group*:

    ```azurecli-interactive
        az group create --help
    ```

6.  Type in the following command and press **Enter** to view a list of possible CLI commands to *list Resource Groups*:

    ```azurecli-interactive
        az group list --help
    ```

7.  Type in the following command and press **Enter** to list all resource groups in the subscription:

    ```azurecli-interactive
        az group list
    ```

8.  Type in the following command and press **Enter** to create a new resource group with the following details:

    -   **Name**: MOD02APPS

    -   **Location**: East US

    <!-- -->

    ```azurecli-interactive
        az group create --name MOD02APPS --location eastus
    ```

9.  Type in the following command and press **Enter** to list all resource groups in the subscription:

    ```azurecli-interactive
        az group list
    ```

10. Close the **Cloud Shell** prompt at the bottom of the portal.

> **Review**: In this exercise, you used the **Azure Portal** to create a resource group directly using the portal interface. You also used the **Cloud Shell** within the portal to create a resource group using the **Azure CLI** tool.

## Exercise 2: Deploy a Minimal Template

#### Task 1: Deploy Storage Account ARM Template

1.  On the left side of the portal, click the **New** link.

2.  At the top of the **New** blade, locate the **Search the Marketplace** field.

3.  Enter the text **Template Deployment** into the search field and press **Enter**.

4.  In the **Everything** search results blade, select the **Template deployment** result.

5.  In the **Template deployment** blade, click the **Create** button.

6.  In the **Custom deployment** blade, click the *Build your own template in the editor* link.

7.  In the **Edit template** blade, locate the text editor and delete the existing template content.

8.  Copy and paste the following ARM template into the template editor:

    ```json
        {
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "resources": [
                {
                    "apiVersion": "2015-06-15",
                    "name": "[concat('stor', uniqueString(resourceGroup().id))]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[resourceGroup().location]",
                    "properties": {
                        "accountType": "Standard_LRS"
                    }
                }
            ]
        }
    ```

9.  Click the **Save** button to persist the template.

10. Back in the **Custom deployment** blade, perform the following actions:

    a.  Leave the **Subscription** field set to its default value.

    b.  In the **Resource group** section, locate the list and select the **MOD02STOR** option.

    c.  In the **Terms and Conditions** section, select the *I agree to the terms and conditions stated above* checkbox.

    d.  Click the **Purchase** button.

11. Wait for the creation task to complete before moving on with this lab.

#### Task 2: View Deployment Metadata

1.  On the left side of the portal, click the **Resource groups** link.

2.  In the **Resource groups** blade, locate and select the **MOD02STOR** *Resource Group* link.

3.  In the **MOD02STOR** blade, locate the **Settings** section on the left side of the blade and click the **Deployments** link.

4.  In the **Deployments** pane, select the latest deployment to view its metadata in a new blade.

5.  Within the deployment blade, observe the information displayed in the **Operation details** section.

> **Review**: In this exercise, you deployed a minimal ARM template that created a deployment using a simple ARM template that deployed a single resource without any input or output values.

## Exercise 3: Deploy a Simple Template

#### Task 1: View ARM Template

1.  On the Taskbar, click the **File Explorer** icon.

2.  In the **File Explorer** window that appears, navigate to the **Allfiles (F):\\Mod02\\Labfiles\\Starter** folder.

3.  Right-click the **deploy.json** file and select the **Open with Code** option to start the **Your favourite IDE** application.

4.  In the **Your favourite IDE** window that appears, observe the content of the JSON file.

5.  At the top of the **Your favourite IDE** window, click the **File** menu and select the **Close Folder** option.

6.  Close the **File Explorer** window.

7.  Return to the **Microsoft Edge** window with the **Azure Portal** open.

#### Task 2: Deploy App Service ARM Template

1.  On the left side of the portal, click the **New** link.

2.  At the top of the **New** blade, locate the **Search the Marketplace** field.

3.  Enter the text **Template Deployment** into the search field and press **Enter**.

4.  In the **Everything** search results blade, select the **Template deployment** result.

5.  In the **Template deployment** blade, click the **Create** button.

6.  In the **Custom deployment** blade, click the *Build your own template in the editor* link.

7.  In the **Edit template** blade, click the **Load file** link.

8.  In the **Open** file dialog that appears, navigate to the **Allfiles (F):\\Mod02\\Labfiles\\Starter** folder.

9.  Select the **deploy.json** file.

10. Click the **Open** button.

11. Back in the **Edit template** blade, click the **Save** button to persist the template.

12. Back in the **Custom deployment** blade, perform the following actions:

    a.  Leave the **Subscription** field set to its default value.

    b.  In the **Resource group** section, locate the list and select the **MOD02APPS** option.

    c.  In the **Function App Name** field, enter a globally unique value. This will be used to create the unique URL for this *Function App*.

    d.  In the **Terms and Conditions** section, select the *I agree to the terms and conditions stated above* checkbox.

    e.  Click the **Purchase** button.

13. Wait for the creation task to complete before moving on with this lab.

#### Task 3: View Deployment Metadata

1.  On the left side of the portal, click the **Resource groups** link.

2.  In the **Resource groups** blade, locate and select the **MOD02APPS** *Resource Group* link.

3.  In the **MOD02APPS** blade, locate the **Settings** section on the left side of the blade and click the **Deployments** link.

4.  In the **Deployments** pane, select the latest deployment to view its metadata in a new blade.

5.  Within the deployment blade, observe the information displayed in the **Inputs** and **Outputs** sections.

> **Review**: In this exercise, you deployed a more sophisticated template that created multiple resources using a combination of parameters, variables and output values.

## Exercise 4: Cleanup Subscription

#### Task 1: Open Cloud Shell

1.  At the top of the portal, click the **Cloud Shell** icon to open a new shell instance.

2.  In the **Cloud Shell** command prompt at the bottom of the portal, type in the following command and press **Enter** to list all resource groups in the subscription:

    ```azurecli-interactive
        az group list
    ```

3.  Type in the following command and press **Enter** to view a list of possible CLI commands to *delete a Resource Group*:

    ```azurecli-interactive
        az group delete --help
    ```

#### Task 2: Delete Resource Groups

1.  Type in the following command and press **Enter** to delete the **MOD02STOR** *Resource Group*:

    ```azurecli-interactive
        az group delete --name MOD02STOR --no-wait --yes
    ```

2.  Type in the following command and press **Enter** to delete the **MOD02APPS** *Resource Group*:

    ```azurecli-interactive
        az group delete --name MOD02APPS --no-wait --yes
    ```

3.  Close the **Cloud Shell** prompt at the bottom of the portal.

