# Lab 4: Integrating SaaS Services Available on the Azure Platform

# Lab Answer Key: Deploying Service Instances as Components of Overall Azure Solutions


## Exercise 1: Deploy Function App and Cognitive Service using ARM Template

#### Task 1: Open the Azure Portal

1.  On the Taskbar, click the **Microsoft Edge** icon.

2.  In the open browser window, navigate to the **Azure Portal** (<https://portal.azure.com>).

3.  Enter the **email address** of your *Microsoft account*.

4.  Click the **Next** button.

5.  Enter the **password** for your *Microsoft account*.

6.  Click the **Sign in** button.

#### Task 2: Deploy Cognitive Service using ARM

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
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "variables": {
            "serviceName": "[concat('cgnt', uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2017-04-18",
                "type": "Microsoft.CognitiveServices/accounts",
                "name": "[variables('serviceName')]",
                "kind": "TextAnalytics",
                "location": "[resourceGroup().location]",
                "sku": {
                    "name": "S1"
                },
                "properties": {}
            }
        ],
        "outputs": {
            "cognitiveEndpointUrl": {
                "type": "string",
                "value": "[reference(variables('serviceName')).endpoint]"
            },
            "cognitiveEndpointKey": {
                "type": "string",
                "value": "[listKeys(variables('serviceName'), '2017-04-18').key1]"
            }
        }
    }
    ```

9.  Click the **Save** button to persist the template.

10. Back in the **Custom deployment** blade, perform the following actions:

    a.  Leave the **Subscription** field set to its default value.

    b.  In the **Resource group** section, select the **Create new** option.

    c.  In the **Resource group** section, enter the value **MOD10COGS** into the empty field.

    d.  In the **Location** field, select the **East US** location.

    e.  In the **Terms and Conditions** section, select the *I agree to the terms and conditions stated above* checkbox.

    f.  Click the **Purchase** button.

11. Wait for the deployment of the DSC configuration to complete before moving on with this lab.

12. On the left side of the portal, click the **Resource groups** link.

13. In the **Resource groups** blade, select the **MOD10COGS** resouce group. On the **Overview** tab, locate the **Deployments** header at the top of the blade and click the hyperlink immediately below the header that indicates the number of successful deployments.

14. In the **Deployments** blade, click the name of the most recent deployment.

15. In the **Microsoft.Template Deployment** blade, record the output values **CognitiveEndpointURL** and **CognitiveEndpointKey**. These values will be used later in the lab.

#### Task 3: Deploy Function App and Code

1.  On the left side of the portal, click the **Create a resource** link.

2.  At the top of the **New** blade, locate the **Search the Marketplace** field.

3.  Enter the text **Function** into the search field and press **Enter**.

4.  In the **Everything** search results blade, select the **Function App** result.

5.  In the **Function App** blade, click the **Create** button.

6.  In the next **Function App** blade, perform the following actions:

    a.  In the **App name** field, enter a globally unique name.

    b.  Leave the **Subscription** field set to its default value.

    c.  In the **Resource group** section, select the **Use existing** option.

    d.  In the **Resource group** section, locate the dropdown list and select the **MOD10COGS** option.

    e.  In the **OS** section, select the **Windows** option.

    f.  In the **Hosting Plan** list, select the **Consumption Plan** option.

    g.  In the **Location** field, select the **East US** location.

    h.  In the **Runtime Stack** leave **.NET** as default

    i.  In the **Storage** section leave the randomly generated storage account name as default.

    j.  In the **Application Insights** section, select the **Off** option.

    k.  Click the **Create** button.

7.  Wait for the creation task to complete before moving on with this lab.

8.  On the left side of the portal, click the **Resource groups** link.

9.  In the **Resource groups** blade, locate and select the **MOD10COGS** *Resource Group* link.

10. In the **MOD10COGS** blade, select the **Function App** you most recently created.

11. In the **Function Apps** blade, click the **Platform features** tab at the top of the blade.

12. In the **Platform features** tab, click the **Application Settings** link in the **General Settings** section.

13. In the **Application settings** tab, scroll down and locate the **Application Settings** section. Click the **Add new setting** link. Perform the following actions:

    a.  In the **Enter a name** field, enter the value **EndpointUrl**.

    b.  In the **Enter a value** field, enter the value you previously recorded for the **CognitiveEndpointURL**.

14. Back in the **Application Settings** section, click the **Add new setting** link. Perform the following actions:

    a.  In the **Enter a name** field, enter the value **EndpointKey**.

    b.  In the **Enter a value** field, enter the value you previously recorded for the **CognitiveEndpointKey**.

15. Click the **Save** button at the top of the **Application settings** tab.

16. Back in the **Function Apps** blade, click the **Platform features** tab at the top of the blade.

17. In the **Platform features** tab, click the **Deployment Center** link in the **Code Deployment** section.

18. In the **Deployments Center** blade that appears, click the **External Repository** link.

19. In the **Deployment option** blade that appears, perform the following actions:

    a.  In the **Repository** field, enter the value **https://github.com/azure-labs/cognitive-services-function**.

    b.  In the **Branch** field, enter the value **master**.

    c.  In the **Repository Type** section, select the **Git** option.

    d.  Click the **Continue** button.

    e.  Click the **Finish** button.

22. Wait for the first deployment to complete.

    > **Note**: You will know the first deployment has completed when you see a successful deployment operation listed on the **Deployments** tab. This tab updates automatically.

#### Task 4: Test Function App

1.  Back in the **Function Apps** blade, click the **Functions** link on the left side of the blade to expand the list of functions.

    > **Note**: You may need to click the **Functions** link twice to refresh the list of functions.

2.  Select the **DetermineLanguage** function from the list of functions.

3.  In the **run.csx** pane that opens, click the **Test** link on the right side of the pane.

4.  In the **Test** popup, perform the following actions:

    a.  In the **Request body** field, enter the following value:

    ```json
    {
        "text": "I stuffed a shirt or two into my old carpet-bag, tucked it under my arm, and started for Cape Horn and the Pacific."
    }
    ```

    a.  Click the **Run** button.

    b.  Observe the output in the **Output** section. The output should correctly identify the language as **en (English)**.

> **Review**: In this exercise, you create a Function App that is uses an Azure Cognitive Services account.

## Exercise 2: Create a Logic App using a Function App

#### Task 1: Create Logic App

1.  On the left side of the portal, click the **Create a resource** link.

2.  At the top of the **New** blade, locate the **Search the Marketplace** field.

3.  Enter the text **Logic** into the search field and press **Enter**.

4.  In the **Everything** search results blade, select the **Logic App** result.

5.  In the **Logic App** blade, click the **Create** button.

6.  In the **Create logic App** blade, perform the following actions:

    a.  In the **Name** field, enter the value **CognitiveWorkflow**.

    b.  Leave the **Subscription** field set to its default value.

    c.  In the **Resource group** section, select the **Use existing** option.

    d.  In the **Resource group** section, locate the dropdown list and select the **MOD10COGS** option.

    e.  In the **Location** field, select the **East US** location.

    f.  In the **Log Analytics** section, select the **Off** option.

    g.  Click the **Create** button.

7.  Wait for the creation task to complete before moving on with this lab.

#### Task 2: Build Logic App Steps

1.  On the left side of the portal, click the **Resource groups** link.

2.  In the **Resource groups** blade, locate and select the **MOD10COGS** *Resource Group* link.

3.  In the **MOD10COGS** blade, select the **CognitiveWorkflow Logic App** you most recently created.

4.  In the **Logic App** blade, you will see the default *Logic Apps Designer* screen. Scroll down and click the **Blank Logic App** option in the **Templates** section.

5.  In the **Logic Apps Designer** pane, click the **Code view** button at the top of the pane.

6.  In the *code view*, view the blank Logic App JSON template:

    ```json
    {
        "definition": {
            "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
            "actions": {},
            "contentVersion": "1.0.0.0",
            "outputs": {},
            "parameters": {},
            "triggers": {}
        }
    }
    ```

7.  Replace the entire JSON template definition with a new template that adds a simple HTTP trigger:

    ```json
    {
        "definition": {
            "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
            "actions": {},
            "contentVersion": "1.0.0.0",
            "outputs": {},
            "parameters": {},
            "triggers": {
                "manual": {
                    "inputs": {
                        "method": "POST",
                        "schema": {
                            "properties": {
                                "text": {
                                    "type": "string"
                                }
                            },
                            "type": "object"
                        }
                    },
                    "kind": "Http",
                    "type": "Request"
                }
            }
        }
    }
    ```

8.  Click the **Designer** button at the top of the pane.

    > **Note**: At this point, you should see a single step in the designer. This is the "trigger" step that begins a workflow.

9.  Click the **+ New Step** button in the designer. Click the **Add an action** button to open the dialog for creating an action.

10. In the **Choose an action** dialog that displays, perform the following actions:

    a.  Enter the value **Azure Functions** into the search field.

    b.  In the search results, select the **Action** named **Azure Functions - Choose an Azure function**.

    c.  In the next set of search results, select the named *Azure Function* instance you created earlier in this lab.

    d.  In the final set of search results, select the **DetermineLanguage** function that will be used for the action.

11. In the **DetermineLanguage** step that is created, perform the following actions:

    a.  Click the **Show advanced options** link to display all options.

    b.  In the **Request Body** field, enter the value **@triggerBody()**.

    c.  In the **Method** list, select the **POST** option.

12. Click the **+ New Step** button in the designer. Click the **Add an action** button to open the dialog for creating an action.

13. In the **Choose an action** dialog that displays, perform the following actions:

    a.  Enter the value **Azure Functions** into the search field.

    b.  In the search results, select the **Action** named **Azure Functions - Choose an Azure function**.

    c.  In the next set of search results, select the named *Azure Function* instance you created earlier in this lab.

    d.  In the final set of search results, select the **DetermineKeyPhrases** function that will be used for the action.

14. In the **DetermineKeyPhrases** step that is created, perform the following actions:

    a.  Click the **Show advanced options** link to display all options.

    b.  In the **Request Body** field, enter the value **@body('DetermineLanguage')**.

    c.  In the **Method** list, select the **POST** option.

15. Click the **+ New Step** button in the designer. Click the **Add an action** button to open the dialog for creating an action.

16. In the **Choose an action** dialog that displays, perform the following actions:

    a.  Enter the value **Response** into the search field.

    b.  In the search results, select the **Action** named **Response -Request**.

17. In the **Response** step that is created, perform the following actions:

    a.  In the **Status Code** field, enter the value **200**.

    b.  In the **Body** field, enter the value **@body('DetermineKeyPhrases')**.

18. At the top of the **Logic Apps Designer** blade, click the **Save** button to persist your workflow.

19. Scroll to the top of the **Logic Apps Designer** area and expand the **When a HTTP request is received** step.

20. Copy the value of the **HTTP POST URL** field. This URL will be used later in this lab.

#### Task 2: Validate Logic App using Python

1.  At the top of the portal, click the **Cloud Shell** icon to open a new shell instance.

2.  In the **Cloud Shell** command prompt at the bottom of the portal, type in the following command and press **Enter** to open the interactive python terminal:

    ```shell
    python
    ```

3.  Type in the following command and press **Enter** to import the **requests** library:

    ```python
    import requests
    ```

4.  Type in the following command and press **Enter** to create a new variable for your Logic App's url replacing the placeholder with the value of your url recorded earlier in this lab:

    ```python
    url = "[Logic App POST Url]"
    ```

    Make sure you replace the **\[Logic App POST Url\]** placeholder with the url for your **Logic App** instance. For example, if your url is **https://prod-16.eastus.logic.azure.com:443/workflows/b3fa4688c0f64345a6bcd7ab60c9f917/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=UPzs4YPwh\_E4nnuUgqeDOBISJmLNwBaXBvzz9WcH9sU**, your command will look like this:

    ```python
    url = "https://prod-16.eastus.logic.azure.com:443/workflows/b3fa4688c0f64345a6bcd7ab60c9f917/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=UPzs4YPwh_E4nnuUgqeDOBISJmLNwBaXBvzz9WcH9sU"
    ```

5.  Type in the following command and press **Enter** to send a HTTP *POST* request to trigger your Logic App workflow:

    ```python
    response = requests.post(url, json={'text': 'Circumambulate the city of a dreamy Sabbath afternoon. Go from Corlears Hook to Coenties Slip, and from thence, by Whitehall, northward.'})
    ```

6.  Type in the following command and press **Enter** to print out the results from the Logic App workflow:

    ```python
    print(response.status_code, response.reason, response.text)
    ```

> **Review**: In this exercise, you created a Logic App that leveraged the Function Apps created earlier in this lab.

## Exercise 3: Cleanup Subscription

#### Task 1: Open Cloud Shell

1.  At the top of the portal, click the **Cloud Shell** icon to open a new shell instance.

2.  In the **Cloud Shell** command prompt at the bottom of the portal, type in the following command and press **Enter** to list all resource groups in the subscription:

    ```azurecli-interactive
    az group list
    ```

#### Task 2: Delete Resource Groups

1.  Type in the following command and press **Enter** to delete the **MOD10COGS** *Resource Group*:

    ```azurecli-interactive
    az group delete --name MOD10COGS --no-wait --yes
    ```

2.  Close the **Cloud Shell** prompt at the bottom of the portal.

#### Task 3: Close Active Applications

1.  Close the currently running **Microsoft Edge** application.

> **Review**: In this exercise, you "cleaned up your subscription" by removing the **Resource Groups** used in this lab.