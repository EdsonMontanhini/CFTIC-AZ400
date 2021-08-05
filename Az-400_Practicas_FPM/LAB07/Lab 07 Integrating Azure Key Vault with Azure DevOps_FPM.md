# Lab 07: Integrating Azure Key Vault with Azure DevOps

# Student lab manual

## Lab overview

Azure Key Vault provides secure storage and management of sensitive data, such as keys, passwords, and certificates. Azure Key Vault includes supports for hardware security modules, as well as a range of encryption algorithms and key lengths. By using Azure Key Vault, you can minimize the possibility of disclosing sensitive data through source code, which is a common mistake made by developers. Access to Azure Key Vault requires proper authentication and authorization, supporting fine grained permissions to its content.

In this lab, you will see how you can integrate Azure Key Vault with an Azure DevOps pipeline by using the following steps:

- create an Azure Key vault to store a MySQL server password as a secret.
- create an Azure service principal to provide access to secrets in the Azure Key vault.
- configure permissions to allow the service principal to read the secret.
- configure pipeline to retrieve the password from the Azure Key vault and pass it on to subsequent tasks.

## Objectives

After you complete this lab, you will be able to:

- Create an Azure Active Directory (Azure AD) service principal.
- Create an Azure key vault.
- Track pull requests through the Azure DevOps pipeline.

## Lab duration

- Estimated time: **40 minutes**

## Instructions

### Before you start

#### Sign in to the lab virtual machine

Ensure that you’re signed in to your Windows 10 virtual machine by using the following credentials:

- Username: **Student**
- Password: **Pa55w.rd**

#### Review applications required for this lab

Identify the applications that you’ll use in this lab:

- Microsoft Edge

#### Prepare an Azure subscription

- Identify an existing Azure subscription or create a new one.
- Verify that you have a Microsoft account or an Azure AD account with the Owner role in the Azure subscription and the Global Administrator role in the Azure AD tenant associated with the Azure subscription. For details, refer to [List Azure role assignments using the Azure portal](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-list-portal) and [View and assign administrator roles in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/roles/manage-roles-portal#view-my-roles).

#### Set up an Azure DevOps organization

If you don’t already have an Azure DevOps organization that you can use for this lab, create one by following the instructions available at [Create an organization or project collection](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/create-organization?view=azure-devops).

### Exercise 0: Configure the lab prerequisites

In this exercise, you will set up the prerequisite for the lab, which consists of the preconfigured Parts Unlimited team project based on an Azure DevOps Demo Generator template.

#### Task 1: Configure the team project

In this task, you will use Azure DevOps Demo Generator to generate a new project based on the **Azure Key Vault** template.

1. On your lab computer, start a web browser and navigate to [Azure DevOps Demo Generator](https://azuredevopsdemogenerator.azurewebsites.net/). This utility site will automate the process of creating a new Azure DevOps project within your account that is prepopulated with content (work items, repos, etc.) required for the lab.

   > **Note**: For more information on the site, see https://docs.microsoft.com/en-us/azure/devops/demo-gen.

2. Click **Sign in** and sign in using the Microsoft account associated with your Azure DevOps subscription.

   ![LAB07-Az400_01](Evidencia/\LAB07-Az400_01.png)

   ![LAB07-Az400_02](Evidencia/\LAB07-Az400_02.png)

   ![LAB07-Az400_03](Evidencia/\LAB07-Az400_03.png)

3. If required, on the **Azure DevOps Demo Generator** page, click **Accept** to accept the permission requests for accessing your Azure DevOps subscription.

   ![LAB07-Az400_04](Evidencia/\LAB07-Az400_04.png)

4. On the **Create New Project** page, in the **New Project Name** textbox, type **Integrating Azure Key Vault with Azure DevOps**, in the **Select organization** dropdown list, select your Azure DevOps organization, and then click **Choose template**.

   ![LAB07-Az400_05](Evidencia/\LAB07-Az400_05.png)

5. On the **Choose a template** page, in the header menu, click **DevOps Labs**, in the list of templates, click the **Azure Key Vault** template, and then click **Select Template**.

   ![LAB07-Az400_06](Evidencia/\LAB07-Az400_06.png)

   ![LAB07-Az400_07](Evidencia/\LAB07-Az400_07.png)

6. Back on the **Create New Project** page, select the checkbox below the **ARM Outputs** label, and click **Create Project**

   ![LAB07-Az400_08](Evidencia/\LAB07-Az400_08.png)

   > **Note**: Wait for the process to complete. This should take about 2 minutes. In case the process fails, navigate to your DevOps organization, delete the project, and try again.

   ![LAB07-Az400_08a](Evidencia/\LAB07-Az400_08a.png)

   ![LAB07-Az400_08b](Evidencia/\LAB07-Az400_08b.png)

7. On the **Create New Project** page, click **Navigate to project**.

   ![LAB07-Az400_08c](Evidencia/\LAB07-Az400_08c.png)

   ![LAB07-Az400_08d](Evidencia/\LAB07-Az400_08d.png)

### Exercise 1: Integrate Azure Key Vault with Azure DevOps

- create an Azure service principal that will provide access to secrets in an Azure Key vault.
- create the Azure Key vault to store a MySQL server password as a secret.
- configure permissions to allow the service principal to read the secret.
- configure pipeline to retrieve the password from the Azure Key vault and pass it on to subsequent tasks.

#### Task 1: Create a service principal

In this task, you will create a service principal by using the Azure CLI.

> **Note**: If you do already have a service principal, you can proceed directly to the next task.

You will need a service principal to deploy an app to an Azure resource from Azure Pipelines. Since we are going to retrieve secrets in a pipeline, we will need to grant permission to the service when we create the Azure Key vault.

A service principal is automatically created by Azure Pipeline when you connect to an Azure subscription from inside a pipeline definition or when you create a new service connection from the project settings page. You can also manually create the service principal from the portal or using Azure CLI and re-use it across projects. It is recommended that you use an existing service principal when you want to have a pre-defined set of permissions.

1. From the lab computer, start a web browser, navigate to the [**Azure Portal**](https://portal.azure.com/), and sign in with the user account that has the Owner role in the Azure subscription you will be using in this lab and has the role of the Global Administrator in the Azure AD tenant associated with this subscription.

   ![LAB07-Az400_09](Evidencia/\LAB07-Az400_09.png)

   ![LAB07-Az400_10](Evidencia/\LAB07-Az400_10.png)

   ![LAB07-Az400_11](Evidencia/\LAB07-Az400_11.png)

   ![LAB07-Az400_12](Evidencia/\LAB07-Az400_12.png)

   ![LAB07-Az400_13](Evidencia/\LAB07-Az400_13.png)

   ![LAB07-Az400_14](Evidencia/\LAB07-Az400_14.png)

2. In the Azure portal, click the **Cloud Shell** icon, located directly to the right of the search textbox at the top of the page.

3. If prompted to select either **Bash** or **PowerShell**, select **Bash**.

> **Note**: If this is the first time you are starting **Cloud Shell** and you are presented with the **You have no storage mounted** message, select the subscription you are using in this lab, and select **Create storage**.

1. From the **Bash** prompt, in the **Cloud Shell** pane, run the following command to create a service principal (replace the `<service-principal-name>` with any unique string of characters consisting of letters and digits):

   CodeCopy

   ```
   az ad sp create-for-rbac --name <service-principal-name>
   ```

   > **Note**: The command will generate a JSON output. Copy the output to text file. You will need it later in this lab.

   ![LAB07-Az400_15](Evidencia/\LAB07-Az400_15.png)

   ![LAB07-Az400_16](Evidencia/\LAB07-Az400_16.png)

2. From the **Bash** prompt, in the **Cloud Shell** pane, run the following commands to retrieve the values of the Azure subscription ID and subscription name attributes:

   CodeCopy

   ```
   az account show --query id --output tsv
   az account show --query name --output tsv
   ```

   > **Note**: Copy both values to a text file. You will need them later in this lab.

![LAB07-Az400_17](Evidencia/\LAB07-Az400_17.png)

#### Task 2: Create an Azure Key vault

In this task, you will create an Azure Key vault by using the Azure portal.

For this lab scenario, we have an app that connects to a MySQL database. We intend to store the password for the MySQL database as a secret in the key vault.

1. In the Azure portal, in the **Search resources, services, and docs** text box, type **Key vaults** and press the **Enter** key.

   ![LAB07-Az400_18](Evidencia/\LAB07-Az400_18.png)

2. On the **Key vaults** blade, click **+ Add**.

   ![LAB07-Az400_19](Evidencia/\LAB07-Az400_19.png)

3. On the **Basics** tab of the **Create key vault** blade, specify the following settings and click **Next: Access policy**:

   | Setting                       | Value                                                        |
   | :---------------------------- | :----------------------------------------------------------- |
   | Subscription                  | the name of the Azure subscription you are using in this lab |
   | Resource group                | the name of a new resource group **az400m07l01-RG**          |
   | Key vault name                | any unique valid name                                        |
   | Region                        | an Azure region close to the location of your lab environment |
   | Pricing tier                  | **Standard**                                                 |
   | Days to retain deleted vaults | **7**                                                        |
   | Purge protection              | **Disable purge protection**                                 |

   ![LAB07-Az400_20](Evidencia/\LAB07-Az400_20.png)

   ![LAB07-Az400_21](Evidencia/\LAB07-Az400_21.png)

4. On the **Access policy** tab of the **Create key vault** blade, click **+ Add Access Policy** to setup a new policy.

   ![LAB07-Az400_22](Evidencia/\LAB07-Az400_22.png)

   > **Note**: You need to secure access to your key vaults by allowing only authorized applications and users. To access the data from the vault, you will need to provide read (Get) permissions to the service principal that you will be using for authentication in the pipeline.

5. On the **Add access policy** blade, click the **None selected** link directly under the **Select principal** label.

   ![LAB07-Az400_23](Evidencia/\LAB07-Az400_23.png)

6. On the **Principal** blade, search for the security principal that you created in the previous exercise, select it, and then click **Select**.

   ![LAB07-Az400_24](Evidencia/\LAB07-Az400_24.png)

   ![LAB07-Az400_25](Evidencia/\LAB07-Az400_25.png)

   ![LAB07-Az400_26](Evidencia/\LAB07-Az400_26.png)

   > **Note**: You can search by name or ID of the principal.

7. Back on the **Add access policy** blade, in the **Secret permissions** drop down list, select checkboxes next to the **Get** and **List** permissions and then click **Add**.

   ![LAB07-Az400_27](Evidencia/\LAB07-Az400_27.png)

   ![LAB07-Az400_28](Evidencia/\LAB07-Az400_28.png)

8. Back on the **Access policy** tab of the **Create key vault** blade, click **Review + create** and, on the **Review + create** blade, click **Create**.

   ![LAB07-Az400_29](Evidencia/\LAB07-Az400_29.png)

   ![LAB07-Az400_30](Evidencia/\LAB07-Az400_30.png)

   > **Note**: Wait for the Azure Key vault to be provisioned. This should take less than 1 minute.

   ![LAB07-Az400_31](Evidencia/\LAB07-Az400_31.png)

9. On the **Your deployment is complete** blade, click **Go to resource**.

   ![LAB07-Az400_32](Evidencia/\LAB07-Az400_32.png)

10. On the Azure Key vault blade, in the vertical menu on the left side of the blade, in the **Settings** section, click **Secrets**.

    ![LAB07-Az400_33](Evidencia/\LAB07-Az400_33.png)

11. On the **Secrets** blade, click **Generate/Import**.

12. On the **Create a secret** blade, specify the following settings and click **Create** (leave others with their default values):

    | Setting        | Value                          |
    | :------------- | :----------------------------- |
    | Upload options | **Manual**                     |
    | Name           | **sqldbpassword**              |
    | Value          | any valid MySQL password value |

![LAB07-Az400_34](Evidencia/\LAB07-Az400_34.png)

![LAB07-Az400_35](Evidencia/\LAB07-Az400_35.png)

![LAB07-Az400_36](Evidencia/\LAB07-Az400_36.png)

![LAB07-Az400_37](Evidencia/\LAB07-Az400_37.png)

#### Task 3: Check the Azure Pipeline

In this task, you will configure the Azure Pipeline to retrieve the secret from the Azure Key vault.

1. On your lab computer, start a web browser and navigate to the Azure DevOps project **Integrating Azure Key Vault with Azure DevOps** you created in the previous exercise.

2. In the vertical navigational pane of the of the Azure DevOps portal, select **Pipelines** and verify that the **Pipelines** pane is displayed.

   ![LAB07-Az400_38](Evidencia/\LAB07-Az400_38.png)

3. On the **Pipelines** pane, click the entry representing the **SmartHotel-CouponManagement-CI** pipeline and, on the **SmartHotel-CouponManagement-CI** pane, click **Run Pipeline**.

   ![LAB07-Az400_39](Evidencia/\LAB07-Az400_39.png)

4. On the **Run pipeline** pane, accept the default settings and click **Run** to trigger a build.

   ![LAB07-Az400_40](Evidencia/\LAB07-Az400_40.png)

   ![LAB07-Az400_41](Evidencia/\LAB07-Az400_41.png)

   ![LAB07-Az400_42](Evidencia/\LAB07-Az400_42.png)

5. In the vertical navigational pane of the of the Azure DevOps portal, in the **Pipelines** section, select **Releases**.

6. On the **SmartHotel-CouponManagement-CD** pane, click **Edit** in the upper right corner.

   ![LAB07-Az400_43](Evidencia/\LAB07-Az400_43.png)

7. On the **All pipelines > SmartHotel-CouponManagement-CD** pane, select the **Task** tab and, in the dropdown menu, select **Dev**.

   ![LAB07-Az400_44](Evidencia/\LAB07-Az400_44.png)

   ![LAB07-Az400_45](Evidencia/\LAB07-Az400_45.png)

   > **Note**: The release definition for **Dev** stage has an **Azure Key Vault** task. This task downloads *Secrets* from an Azure Key Vault. You will need to point to the subscription and the Azure Key Vault resource created earlier in the lab.

   > **Note**: You need to authorize the pipeline to deploy to Azure. Azure pipelines can automatically create a service connection with a new service principal, but we want to use the one we created earlier.

8. Select the **Azure Key Vault** task and, on the right side, in the **Azure Key Vault** task properties, next to the **Azure subscription** label, click **Manage**. This will open another browser tab displaying the **Service connections** pane in the Azure DevOps portal.

   ![LAB07-Az400_46](Evidencia/\LAB07-Az400_46.png)

9. On the **Service connections** pane, click **New Service connection**.

   ![LAB07-Az400_47](Evidencia/\LAB07-Az400_47.png)

10. On the **New service connection** pane, select the **Azure Resource Manager** option, click **Next**, select **Service Principal (manual)**, and click **Next** again.

    ![LAB07-Az400_48](Evidencia/\LAB07-Az400_48.png)

    ![LAB07-Az400_49](Evidencia/\LAB07-Az400_49.png)

11. On the **New service connection** pane, specify the following settings, using the information you copied to a text file in the first task of this exercise following creation of the service principal by using Azure CLI:

    - Subscription Id: the value you obtained by running `az account show --query id --output tsv`

    - Subscription Name: the value you obtained by running `az account show --query name --output tsv`

    - Service Principal Id: the value labeled **appId** in the output generated by running `az ad sp create-for-rbac --name <service-principal-name>`

    - Service Principal key: the value labeled **password** in the output generated by running `az ad sp create-for-rbac --name <service-principal-name>`

    - TenantId: the value labeled **tenant** in the output generated by running `az ad sp create-for-rbac --name <service-principal-name>`

      ![LAB07-Az400_50](Evidencia/\LAB07-Az400_50.png)

      ![LAB07-Az400_51](Evidencia/\LAB07-Az400_51.png)

      ![LAB07-Az400_52](Evidencia/\LAB07-Az400_52.png)

      ![LAB07-Az400_53](Evidencia/\LAB07-Az400_53.png)

      

12. On the **New service connection** pane, click **Verify** to determine whether the information you provided is valid.

    ![LAB07-Az400_54](Evidencia/\LAB07-Az400_54.png)

    

13. Once you receive the **Verification Succeeded** response, in the **Service connection name** textbox, type **kv-service-connection** and click **Verify and Save**.

    ![LAB07-Az400_55](Evidencia/\LAB07-Az400_55.png)

    ![LAB07-Az400_56](Evidencia/\LAB07-Az400_56.png)

    

14. Switch back to the web browser tab displaying the pipeline definition and the **Azure Key Vault** task.

    ![LAB07-Az400_59](Evidencia/\LAB07-Az400_59.png)

15. With the **Azure Key Vault** task selected, on the **Azure Key Vault** pane, click the **Refresh** button, in the **Azure subscription** dropdown list, select the **kv-service-connection** entry, in the **Key vault** dropdown list, select the entry representing the Azure Key vault you created in the first task, and, in the **Secrets filter** textbox, type **sqldbpassword**. Finally, expand the **Output Variables** section and, in the **Reference name** textbox, type **sqldbpassword**.

    ![LAB07-Az400_60](Evidencia/\LAB07-Az400_60.png)

    ![LAB07-Az400_61](Evidencia/\LAB07-Az400_61.png)

    ![LAB07-Az400_62](Evidencia/\LAB07-Az400_62.png)

    ![LAB07-Az400_63](Evidencia/\LAB07-Az400_63.png)

    ![LAB07-Az400_63_](Evidencia/\LAB07-Az400_63_.png)

    > **Note**: At runtime, Azure Pipelines will fetch the latest value of the secret and set it as the task variable **$(sqldbpassword)**. The tasks can consumed by the subsequent tasks by referencing that variable.

16. To verify this, select the next task, **Azure Deployment**, which deploys an ARM template and review the content of the **Override template parameters** textbox.

    CodeCopy

    ```
    -webAppName $(webappName) -mySQLAdminLoginName "azureuser" -mySQLAdminLoginPassword $(sqldbpassword)
    ```

    > **Note**: The **Override template parameters** content references the **sqldbpassword** variable to set the mySQL admin password. This will provision the MySQL database defined in the ARM template using the password that you have specified in the key vault.

    ![LAB07-Az400_64](Evidencia/\LAB07-Az400_64.png)

    ![LAB07-Az400_65](Evidencia/\LAB07-Az400_65.png)

    ![LAB07-Az400_66](Evidencia/\LAB07-Az400_66.png)

17. You may complete the pipeline definition by specifying the subscription (if new subscription is used in the project, click on **Authorize** )and location for the task. **Repeat** the same for the last task in the pipeline **Azure App Service Deploy** (choose the subscription from the **Available Azure service connection** section in the dropdown).

    ![LAB07-Az400_67](Evidencia/\LAB07-Az400_67.png)

    > **Note**: In the Azure subscription dropdown list, you will see **Available Azure service connections** for those susbcriptions that have already been authorized to be connected to Azure. If you select the authorized subscription again (from **Available Azure subscriptions** list) and try to **Authorize**, the process will fail.

18. Finally, **Save** and click on **Create a new release** > **Create** (leave defaults) to start the deployment.

    ![LAB07-Az400_68](Evidencia/\LAB07-Az400_68.png)

    ![LAB07-Az400_69](Evidencia/\LAB07-Az400_69.png)

    ![LAB07-Az400_70](Evidencia/\LAB07-Az400_70.png)

    ![LAB07-Az400_71](Evidencia/\LAB07-Az400_71.png)

    ![LAB07-Az400_72](Evidencia/\LAB07-Az400_72.png)

    

19. Make sure your pipeline runs successfully and once finished, review the created resources by opening the resource group **az400m07l01-RG** in the Azure Portal . Open the **App Service** and browse it **(Overview -> Browse)**, to see the published website.

    ![LAB07-Az400_72a](Evidencia/\LAB07-Az400_72a.png)

    

    ![LAB07-Az400_72b](Evidencia/\LAB07-Az400_72b.png)

    ![LAB07-Az400_72c](Evidencia/\LAB07-Az400_72c.png)

    ![LAB07-Az400_72c1](Evidencia/\LAB07-Az400_72c1.png)

    ![LAB07-Az400_72d](Evidencia/\LAB07-Az400_72d.png)

    ![LAB07-Az400_72e](Evidencia/\LAB07-Az400_72e.png)

    The new release fails in the Password validation within Azure Deployment phase because the created password **is Not Complex**.

    ![LAB07-Az400_73](Evidencia/\LAB07-Az400_73.png)

    Navigate to lab07kvfpm (Key Vault) within az400m07l01-RG resource group to verify the password (Secret Value) and regenerates it if is necessary.

    ![LAB07-Az400_74](Evidencia/\LAB07-Az400_74.png)

    ![LAB07-Az400_75](Evidencia/\LAB07-Az400_75.png)

    ![LAB07-Az400_76](Evidencia/\LAB07-Az400_76.png)

    ![LAB07-Az400_77](Evidencia/\LAB07-Az400_77.png)

    ![LAB07-Az400_78](Evidencia/\LAB07-Az400_78.png)

    

    ![LAB07-Az400_79](Evidencia/\LAB07-Az400_79.png)

    Delete sqldbpassword

    ![LAB07-Az400_80](Evidencia/\LAB07-Az400_80.png)

    ![LAB07-Az400_81](Evidencia/\LAB07-Az400_81.png)

    ![LAB07-Az400_82](Evidencia/\LAB07-Az400_82.png)

    Generate the Secret Value again.

    ![LAB07-Az400_83](Evidencia/\LAB07-Az400_83.png)

    ![LAB07-Az400_83a](Evidencia/\LAB07-Az400_83a.png)

    There is a problem generating the key.

    Check the subscription and and MySQL resource provider

    ![LAB07-Az400_84](Evidencia/\LAB07-Az400_84.png)

    ![LAB07-Az400_85](Evidencia/\LAB07-Az400_85.png)

    The subscription is valid and active.

    ![LAB07-Az400_86](Evidencia/\LAB07-Az400_86.png)

    The Microsoft:DBforMySQL is registered.

    ![LAB07-Az400_87](Evidencia/\LAB07-Az400_87.png)

    The conflict was solved changing the secret name by a new one (sqldbpassword07) because not allowed modify the password with the old one (sqldbpassword)

    ![LAB07-Az400_89](Evidencia/\LAB07-Az400_89.png)

    The secret was succesfully created.

    ![LAB07-Az400_90](Evidencia/\LAB07-Az400_90.png)

    Now, it is necessary navigate to the Pipeline to change the secrets filter in the Azure Key Vault in Dev- Tasks and Run the Pipeline again.

    ![LAB07-Az400_90a](Evidencia/\LAB07-Az400_90a.png)

    ![LAB07-Az400_91](Evidencia/\LAB07-Az400_91.png)

    ![LAB07-Az400_92](Evidencia/\LAB07-Az400_92.png)

    ![LAB07-Az400_92a](Evidencia/\LAB07-Az400_92a.png)

    ![LAB07-Az400_92a1](Evidencia/\LAB07-Az400_92a1.png)

    ![LAB07-Az400_92a2](Evidencia/\LAB07-Az400_92a2.png)

    ![LAB07-Az400_92a3](Evidencia/\LAB07-Az400_92a3.png)

    

    ![LAB07-Az400_92d](Evidencia/\LAB07-Az400_92d.png)

    ![LAB07-Az400_92d1](Evidencia/\LAB07-Az400_92d1.png)

    ![LAB07-Az400_92d2](Evidencia/\LAB07-Az400_92d2.png)

    ![LAB07-Az400_92d3](Evidencia/\LAB07-Az400_92d3.png)

    ![LAB07-Az400_92d4](Evidencia/\LAB07-Az400_92d4.png)

    ![LAB07-Az400_92e](Evidencia/\LAB07-Az400_92e.png)

    The taks continues failling, so there is to check if it is necessary to change the name in more fields.

    ![LAB07-Az400_92f](Evidencia/\LAB07-Az400_92f.png)

    In Azure Key Vault, is changed the name of reference name by sqldbpassword07 as well.

![LAB07-Az400_93](Evidencia/\LAB07-Az400_93.png)

![LAB07-Az400_94](Evidencia/\LAB07-Az400_94.png)

![LAB07-Az400_95](Evidencia/\LAB07-Az400_95.png)

![LAB07-Az400_96](Evidencia/\LAB07-Az400_96.png)

Create a new release.

![LAB07-Az400_97](Evidencia/\LAB07-Az400_97.png)

![LAB07-Az400_98](Evidencia/\LAB07-Az400_98.png)

![LAB07-Az400_99](Evidencia/\LAB07-Az400_99.png)

![LAB07-Az400_100](Evidencia/\LAB07-Az400_100.png)

![LAB07-Az400_101](Evidencia/\LAB07-Az400_101.png)

Now, there is a problem with the deployment.

It is necessary to change the secret name in the Template within Azure Deployment - Dev task.

![LAB07-Az400_102](Evidencia/\LAB07-Az400_102.png)

![LAB07-Az400_103](Evidencia/\LAB07-Az400_103.png)

![LAB07-Az400_104](Evidencia/\LAB07-Az400_104.png)

![LAB07-Az400_105](Evidencia/\LAB07-Az400_105.png)

![LAB07-Az400_106](Evidencia/\LAB07-Az400_106.png)

![LAB07-Az400_107](Evidencia/\LAB07-Az400_107.png)

![LAB07-Az400_108](Evidencia/\LAB07-Az400_108.png)

Run the Release-3.

![LAB07-Az400_109](Evidencia/\LAB07-Az400_109.png)

![LAB07-Az400_110](Evidencia/\LAB07-Az400_110.png)

![LAB07-Az400_111](Evidencia/\LAB07-Az400_111.png)

![LAB07-Az400_111b](Evidencia/\LAB07-Az400_111b.png)

![LAB07-Az400_112](Evidencia/\LAB07-Az400_112.png)

![LAB07-Az400_113](Evidencia/\LAB07-Az400_113.png)

The deployment was done successfully but it was not created in the suitable resource group (azurekeyvaultlab, instead az400m0701-RG)

![LAB07-Az400_115](Evidencia/\LAB07-Az400_115.png)

![LAB07-Az400_116](Evidencia/\LAB07-Az400_116.png)

Move the resources from azurekeyvaultlab to az400m07l01-RG

![LAB07-Az400_117](Evidencia/\LAB07-Az400_117.png)

![LAB07-Az400_118](Evidencia/\LAB07-Az400_118.png)

![LAB07-Az400_119](Evidencia/\LAB07-Az400_119.png)

![LAB07-Az400_120](Evidencia/\LAB07-Az400_120.png)

Once they have been moved, delete the Azure Database for MySQL and the App Service from the resource group.

![LAB07-Az400_121](Evidencia/\LAB07-Az400_121.png)

![LAB07-Az400_122](Evidencia/\LAB07-Az400_122.png)

![LAB07-Az400_123](Evidencia/\LAB07-Az400_123.png)

![LAB07-Az400_124](Evidencia/\LAB07-Az400_124.png)

Now, delete the azurekeyvaultlab resource group and perform the deploy of the pipeline again, in the suitable resource group.

![LAB07-Az400_125](Evidencia/\LAB07-Az400_125.png)

![LAB07-Az400_126](Evidencia/\LAB07-Az400_126.png)

![LAB07-Az400_127](Evidencia/\LAB07-Az400_127.png)

![LAB07-Az400_128](Evidencia/\LAB07-Az400_128.png)

![LAB07-Az400_129](Evidencia/\LAB07-Az400_129.png)

The logs shows that the web packaged was succesfully deployed to App Service of the Azure Portal.

![LAB07-Az400_130](Evidencia/\LAB07-Az400_130.png)

![LAB07-Az400_131](Evidencia/\LAB07-Az400_131.png)

In the az400m07l01-RG of Azure Portal, we can see the resources that have been deployed:

Azure Database for MySQL.

App Service with App Service Plan.

![LAB07-Az400_132](Evidencia/\LAB07-Az400_132.png)

Open the **App Service** and browse it **(Overview -> Browse)**, to see the published website.

![LAB07-Az400_133](Evidencia/\LAB07-Az400_133.png)

![LAB07-Az400_133a](Evidencia/\LAB07-Az400_133a.png)

![LAB07-Az400_134](Evidencia/\LAB07-Az400_134.png)

![LAB07-Az400_134a](Evidencia/\LAB07-Az400_134a.png)

![LAB07-Az400_135](Evidencia/\LAB07-Az400_135.png)

![LAB07-Az400_136](Evidencia/\LAB07-Az400_136.png)

![LAB07-Az400_137](Evidencia/\LAB07-Az400_137.png)

![LAB07-Az400_138](Evidencia/\LAB07-Az400_138.png)

![LAB07-Az400_139](Evidencia/\LAB07-Az400_139.png)

### Exercise 2: Remove the Azure lab resources

In this exercise, you will remove the Azure resources provisione in this lab to eliminate unexpected charges.

> **Note**: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges.

#### Task 1: Remove the Azure lab resources

In this task, you will use Azure Cloud Shell to remove the Azure resources provisioned in this lab to eliminate unnecessary charges.

1. In the Azure portal, open the **Bash** shell session within the **Cloud Shell** pane.

2. List all resource groups created throughout the labs of this module by running the following command:

   ShellCopy

   ```sh
   az group list --query "[?starts_with(name,'az400m07l01-RG')].name" --output tsv
   ```

   ![LAB07-Az400_140](Evidencia/\LAB07-Az400_140.png)

3. Delete all resource groups you created throughout the labs of this module by running the following command:

   ShellCopy

   ```sh
   az group list --query "[?starts_with(name,'az400m07l01-RG')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

   > **Note**: The command executes asynchronously (as determined by the –nowait parameter), so while you will be able to run another Azure CLI command immediately afterwards within the same Bash session, it will take a few minutes before the resource groups are actually removed.

![LAB07-Az400_141](Evidencia/\LAB07-Az400_141.png)

![LAB07-Az400_142](Evidencia/\LAB07-Az400_142.png)

#### Review

In this lab, you integrated Azure Key Vault with an Azure DevOps pipeline by using the following steps:

- created an Azure Key vault to store a MySQL server password as a secret.
- created an Azure service principal to provide access to secrets in the Azure Key vault.
- configured permissions to allow the service principal to read the secret.
- configured pipeline to retrieve the password from the Azure Key vault and pass it on to subsequent tasks.