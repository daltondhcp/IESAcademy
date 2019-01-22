# Building Azure IaaS-Based Server Applications

# Lab Answer Key: Deploying Infrastructure Workloads to Azure

## Before we start

## Exercise 1: Deploy a Virtual Machine PowerShell Desired State Configuration (DSC) using ARM

#### Task 1: Open the Azure Portal

1.  On the Taskbar, click the **Microsoft Edge** icon.

2.  In the open browser window, navigate to the **Azure Portal** (<https://portal.azure.com>).

3.  Enter the **email address** of your *Microsoft account*.

4.  Click the **Next** button.

5.  Enter the **password** for your *Microsoft account*.

6.  Click the **Sign in** button.

#### Task 2: Create a Windows Virtual Machine

1.  On the left side of the portal, click the **+ Create a resource** link.

2.  At the top of the **New** blade, locate the **Search the Marketplace** field.

3.  Enter the text **Windows Server 2016** into the search field and press **Enter**.

4.  In the **Everything** search results blade, select the **Windows Server 2016 Datacenter** result.

5.  In the **Windows Server 2016 Datacenter** blade, click the **Create** button.

6.  In the **Create a virtual machine** blade, perform the following steps:

    a.  Leave the **Subscription** field set to its default value.

    b.  In the **Resource group** section, click the **Create new** link.

    c.  In the **Resource group** section, enter the value **MOD03VDSC** into the empty field.

    d.  In the **Virtual machine name** field, enter the value **autoconfigvm**.

    e.  In the **Region** field, select the **East US** location.

    f.  In the **Image** field, make sure the **Windows Server 2016 Datacenter** option is selected.

    g.  In the **Size** field, click the **Change size** link.

    h.  In the **Select a VM size** select the **DS2\_V2** size and click the **Select** button.

    i.  In the **User name** field, enter the value **Student**.

    j.  In the **Password** and **Confirm password** fields, enter the value **StudentPa55w.rd**.

    k.  In the **Public inboud ports** option, select the **Allow selected ports** option.

    l.  In the **Selected inbound ports** list, select the **RDP (3389)** option.

    m.  Click the **Next: Disks \>** button.

7.  In the **Disks** tab perform the following steps:

    a.  In the **OS disk type** option, make sure the **Premium SSD** option is selected.

    b.  Under **Advanced**  in the **Use unmanaged disks** option, select the option **No**.

    c.  Click the **Review + create** button.

8.  The **Review & Create** tab, review all the select options and then click **Create** to create the virtual machine using your specified configuration..

9.  Wait for the creation task to complete before moving on with this lab.

#### Task 3: View DSC Configuration

1.  On the Taskbar, click the **File Explorer** icon.

2.  In the **File Explorer** window that appears, navigate to the **Allfiles (F):\\Mod03\\Labfiles\\Starter** folder.

3.  Right-click the **IISWebServer.zip** file and select the **Extract All...** option.

4.  In the **Extract Compressed (Zipped) Folders** dialog, perform the following actions:

    a.  In the **Files will be extracted to this folder:** field, enter the value **(F):\\Mod03\\Labfiles\\Starter\\IISWebServer**.

    b.  Ensure that the **Show extracted files when complete** checkbox is selected.

    c.  Click the **Extract** button.

5.  In the new **File Explorer** window that appears, right-click the **IISWebServer.ps1** file and select the **Open with Code** option to start the **Your favourite IDE** application.

6.  In the **Your favourite IDE** window that appears, observe the content of the PowerShell script.

7.  At the top of the **Your favourite IDE** window, click the **File** menu and select the **Close Window** option.

8.  Close both **File Explorer** windows.

9.  Return to the **Microsoft Edge** window with the **Azure Portal** open.

#### Task 4: Upload DSC Configuration Module to Azure Storage

1.  On the left side of the portal, click the **Create a resource** link.

2.  At the top of the **New** blade, locate the **Search the Marketplace** field.

3.  Enter the text **Storage** into the search field and press **Enter**.

4.  In the **Everything** search results blade, select the **Storage account - blob, file, table, queue** result.

5.  In the **Storage account - blob, file, table, queue** blade, click the **Create** button.

6.  In the **Create storage account** blade, perform the following actions:

    a.  In the **Name** field, enter a globally unique name.

    b.  Leave the **Subscription** field set to its default value.

    c.  In the **Resource group** section, locate the dropdown list and select the **MOD03VDSC** option.

    d.  In the **Location** field, select the **East US** location.

    e.  Leave all other fields set to their default values.

    f.  Click the **Review + create** button.

7.  Wait for the creation task to complete before moving on with this lab.

8.  On the left side of the portal, click the **Resource groups** link.

9.  In the **Resource groups** blade, locate and select the **MOD03VDSC** *Resource Group* link.

10. In the **MOD03VDSC** blade, select the **Storage Account** you recently created.

11. In the **Storage account** blade, click the **Blobs** link in the center of the blade.

12. In the **Blob service** blade, click the **Container** button at the top of the blade.

13. In the **New container** dialog that appears, perform the following actions:

    a.  In the **Name** field, enter the value **config**.

    b.  In the **Public access level** list, select the **Blob (anonymous read access for blobs only)** option.

    c.  Click the **OK** button.

14. Back in the **Blob service** blade, select the new **config** container from the list of containers.

15. In the *config* blade, click the **Upload** button at the top of the blade.

16. In the **Upload blob** popup, perform the following actions:

    a.  In the **Files** field, click the *blue folder* button to the right of the field.

    b.  In the **Open** file dialog that appears, navigate to the **Allfiles (F):\\Mod03\\Labfiles\\Starter** folder.

    c.  Select the **IISWebServer.zip** file.

    d.  Click the **Open** button to close the dialog and return to the **Upload blob** popup.

    e.  Click the **Upload** button.

17. Return to the **config** blade, select the **IISWebServer.zip** blob in the list of blobs.

18. In the **Blob properties** popup that appears, locate and record the value of the **URL** property. This URL will be used later in this lab.

#### Task 5: Deploy a Virtual Machine ARM Template using PowerShell DSC Extension

1.  On the left side of the portal, click the **Create a resource** link.

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
            "parameters": {
                "virtualMachineName": {
                    "type": "string",
                    "defaultValue": "autoconfigvm"
                },
                "configurationModuleUrl": {
                    "type": "string"
                },
                "extensionFunction": {
                    "type": "string",
                    "defaultValue": "IISWebServer.ps1\\IISWebServer"
                }
            },
            "resources": [
                {
                    "apiVersion": "2017-03-30",
                    "type": "Microsoft.Compute/virtualMachines/extensions",
                    "name": "[concat(parameters('virtualMachineName'), '/dscExtension')]",
                    "location": "[resourceGroup().location]",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.19",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "ModulesUrl": "[parameters('configurationModuleUrl')]",
                            "ConfigurationFunction": "[parameters('extensionFunction')]",
                            "Properties": {
                                "MachineName": "[parameters('virtualMachineName')]"
                            }
                        },
                        "protectedSettings": null
                    }
                }
            ]
        }
    ```

9.  Click the **Save** button to persist the template.

10. Back in the **Custom deployment** blade, perform the following actions:

    a.  Leave the **Subscription** field set to its default value.

    b.  In the **Resource group** section, locate the dropdown list and select the **MOD03VDSC** option.

    c.  Leave the **Virtual Machine Name** field set to its default value: **autoconfigvm**.

    d.  In the **Configuration Module Url** field, enter the URL that you recorded for the uploaded blob earlier in this lab.

    e.  Leave the **Extension Function** field set to its default value: **IISWebServer.ps1\\IISWebServer**.

    f.  In the **Terms and Conditions** section, select the *I agree to the terms and conditions stated above* checkbox.

    g.  Click the **Purchase** button.

11. Wait for the deployment of the DSC configuration to complete before moving on with this lab.

    > **Note**: DSC configuration deployment can take up to ten minutes.

#### Task 6: Validate Virtual Machine Instance is Serving Web Content

1.  On the left side of the portal, click the **Resource groups** link.

2.  In the **Resource groups** blade, locate and select the **MOD03VDSC** *Resource Group* link.

3.  In the **MOD03VDSC** blade, select the **Network Security Group** you recently created.

4.  In the **Network security group** blade, locate the **Settings** section on the left side of the blade and click the **Inbound security rules** link.

5.  In the **Inbound security rules** pane, click the **Add** button at the top of the blade.

6.  In the **Add inbound security rule** pane, perform the following actions:

    a.  In the **Source** list, select the **Any** value.

    b.  In the **Source port ranges** field, enter the value **\***.

    c.  In the **Destination** list, select the **Any** value.

    d.  In the **Destination port ranges** field, enter the value **80**.

    e.  In the **Protocol** section, select the **TCP** option.

    f.  In the **Action** section, select the **Allow** option.

    g.  In the **Priority** field, enter the value **1001**.

    h.  In the **Name** field, enter the value **HTTP**.

    i.  In the **Description** field, enter the value **Web requests for HTTP**.

    j.  Click the **Add** button.

7.  On the left side of the portal, click the **Resource groups** link.

8.  In the **Resource groups** blade, locate and select the **MOD03VDSC** *Resource Group* link.

9.  In the **MOD03VDSC** blade, select the **Virtual Machine** you recently created.

10. In the **Virtual machine** blade, locate the **Public IP address** field at the top of the blade. Copy the *value* of this field.

11. In a new browser tab, navigate to the IP address you copied in the previous step.

12. Observe the *IIS* placeholder webpage.

13. Close the additional browser tab and return to the browser tab with the **Azure Portal** currently active.

> **Review**: In this exercise, you created a **Virtual Machine** manually and then used a **PowerShell DSC** configuration module to apply changes to the Virtual Machine in an unattended manner.

## Exercise 2: Deploy a Virtual Machine Scale Set (VMSS) using PowerShell Desired State Configuration (DSC)

#### Task 1: View ARM Template

1.  On the Taskbar, click the **File Explorer** icon.

2.  In the **File Explorer** window that appears, navigate to the **Allfiles (F):\\Mod03\\Labfiles\\Starter** folder.

3.  Right-click the **vmss.json** file and select the **Open with Code** option to start the **Your favourite IDE** application.

4.  In the **Your favourite IDE** window that appears, observe the content of the JSON file.

5.  At the top of the **Your favourite IDE** window, click the **File** menu and select the **Close Window** option.

6.  Close the **File Explorer** window.

7.  Return to the **Microsoft Edge** window with the **Azure Portal** open.

#### Task 2: Deploy a Windows VMSS using ARM

1.  On the left side of the portal, click the **Create a resource** link.

2.  At the top of the **New** blade, locate the **Search the Marketplace** field.

3.  Enter the text **Template Deployment** into the search field and press **Enter**.

4.  In the **Everything** search results blade, select the **Template deployment** result.

5.  In the **Template deployment** blade, click the **Create** button.

6.  In the **Custom deployment** blade, click the *Build your own template in the editor* link.

7.  In the **Edit template** blade, click the **Load file** link.

8.  In the **Open** file dialog that appears, navigate to the **Allfiles (F):\\Mod03\\Labfiles\\Starter** folder.

9.  Select the **vmss.json** file.

10. Click the **Open** button.

11. Back in the **Edit template** blade, click the **Save** button to persist the template.

12. Back in the **Custom deployment** blade, perform the following actions:

    a.  Leave the **Subscription** field set to its default value.

    b.  In the **Resource group** section, select the **Create new** option.

    c.  In the **Resource group** section, enter the value **MOD03VMSS** into the empty field.

    d.  In the **Instance Count** field, enter the value **2**.

    e.  Leave the **Overprovision** field set to its default value: **true**.

    f.  In the **Configuration Module Url** field, enter the URL that you recorded for the uploaded blob earlier in this lab.

    g.  In the **Terms and Conditions** section, select the *I agree to the terms and conditions stated above* checkbox.

    h.  Click the **Purchase** button.

13. Wait for the creation task to complete before moving on with this lab.

#### Task 3: Validate VMSS Instances are Serving Web Content

1.  On the left side of the portal, click the **Resource groups** link.

2.  In the **Resource groups** blade, locate and select the **MOD03VMSS** *Resource Group* link.

3.  In the **MOD03VMSS** blade, select the **Virtual Machine Scale Set** you recently created.

4.  In the **Virtual machine scale set** blade, locate the **Public IP address** field at the top of the blade. Copy the *value* of this field.

5.  In a new browser tab, navigate to the IP address you copied in the previous step.

6.  Observe the *IIS* placeholder webpage.

7.  Close the additional browser tab and return to the browser tab with the **Azure Portal** currently active.

> **Review**: In this exercise, you created a **Virtual Machine Scale Set** and configured the individual instances using PowerShell DSC to facilitate the VMSS service's ability to provision and de-provision **Virtual Machine** instances automatically.

## Exercise 3: Cleanup Subscription

#### Task 1: Open Cloud Shell

1.  At the top of the portal, click the **Cloud Shell** icon to open a new shell instance.

2.  In the **Cloud Shell** command prompt at the bottom of the portal, type in the following command and press **Enter** to list all resource groups in the subscription:

    ```azurecli-interactive
        az group list
    ```

#### Task 2: Delete Resource Groups

1.  Type in the following command and press **Enter** to delete the **MOD03VDSC** *Resource Group*:

    ```azurecli-interactive
        az group delete --name MOD03VDSC --no-wait --yes
    ```

2.  Type in the following command and press **Enter** to delete the **MOD03VMSS** *Resource Group*:

    ```azurecli-interactive
        az group delete --name MOD03VMSS --no-wait --yes
    ```

3.  Close the **Cloud Shell** prompt at the bottom of the portal.

#### Task 3: Close Active Applications

1.  Close the currently running **Microsoft Edge** application.

> **Review**: In this exercise, you "cleaned up your subscription" by removing the **Resource Groups** used in this lab.