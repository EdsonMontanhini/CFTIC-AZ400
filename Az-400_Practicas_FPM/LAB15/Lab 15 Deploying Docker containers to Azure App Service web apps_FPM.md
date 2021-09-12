# Lab 15: Deploying Docker containers to Azure App Service web apps

# Student lab manual

## Lab overview

In this lab, you will learn how to use an Azure DevOps CI/CD pipeline to build a custom Docker image, push it to Azure Container Registry, and deploy it as a container to Azure App Service.

## Objectives

After you complete this lab, you will be able to:

- Build a custom Docker image by using an Microsoft hosted Linux agent
- Push an image to Azure Container Registry
- Deploy a Docker image as a container to Azure App Service by using Azure DevOps

## Lab duration

- Estimated time: **60 minutes**

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

- Verify that you have a Microsoft account or an Azure AD account with the Contributor or the Owner role in the Azure subscription. For details, refer to [List Azure role assignments using the Azure portal](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-list-portal) and [View and assign administrator roles in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/roles/manage-roles-portal#view-my-roles).

  ![LAB15-Az400_01](Evidencia/LAB15-Az400_01.png)

  ![LAB15-Az400_02](Evidencia/LAB15-Az400_02.png)

#### Set up an Azure DevOps organization

If you don’t already have an Azure DevOps organization that you can use for this lab, create one by following the instructions available at [Create an organization or project collection](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/create-organization?view=azure-devops).

### Exercise 1: Configure the lab prerequisites

In this exercise, you will set up the prerequisites for the lab, which consist of a team project based on an Azure DevOps Demo Generator template and Azure resources, including an Azure App Service web app, an Azure Container Registry instance, and an Azure SQL database.

#### Task 1: Configure the team project

In this task, you will use Azure DevOps Demo Generator to generate a new project based on the Docker template.

> **Note**: The Docker template-based project builds and deploys a containerized ASP.NET Core app to Azure App Service

1. On your lab computer, start a web browser and navigate to [Azure DevOps Demo Generator](https://azuredevopsdemogenerator.azurewebsites.net/). This utility site will automate the process of creating a new Azure DevOps project within your account that is prepopulated with content (work items, repos, etc.) required for the lab.

   > **Note**: For more information on the site, see https://docs.microsoft.com/en-us/azure/devops/demo-gen.

2. Click **Sign in** and sign in using the Microsoft account associated with your Azure DevOps subscription.

   ![LAB15-Az400_03](Evidencia/LAB15-Az400_03.png)

3. If required, on the **Azure DevOps Demo Generator** page, click **Accept** to accept the permission requests for accessing your Azure DevOps subscription.

4. On the **Create New Project** page, in the **New Project Name** textbox, type **Deploying Docker containers to Azure App Service web apps**, in the **Select organization** dropdown list, select your Azure DevOps organization, and then click **Choose template**.

   ![LAB15-Az400_04](Evidencia/LAB15-Az400_04.png)

5. In the list of templates, in the toolbar, click **DevOps Labs**, select the **DevOps Labs** header, click the **Docker** template, and then click **Select Template**.

   ![LAB15-Az400_05](Evidencia/LAB15-Az400_05.png)

6. Back on the **Create New Project** page, if prompted to install missing extensions, select the checkbox below the **Docker Integration** label and click **Create Project**.

   ![LAB15-Az400_06](Evidencia/LAB15-Az400_06.png)

   ![LAB15-Az400_07](Evidencia/LAB15-Az400_07.png)

   > **Note**: Wait for the process to complete. This should take about 2 minutes. In case the process fails, navigate to your DevOps organization, delete the project, and try again.

7. On the **Create New Project** page, click **Navigate to project**.

   ![LAB15-Az400_08](Evidencia/LAB15-Az400_08.png)

   ![LAB15-Az400_09](Evidencia/LAB15-Az400_09.png)

#### Task 2: Create Azure resources

In this task, you will use Azure Cloud Shell to create Azure resources required in this lab:

- Azure Container Registry
- Azure Web App for Containers
- Azure SQL Database

1. From the lab computer, start a web browser, navigate to the [**Azure Portal**](https://portal.azure.com/), and sign in with the user account that has the Owner role in the Azure subscription you will be using in this lab and has the role of the Global Administrator in the Azure AD tenant associated with this subscription.

2. In the web browser displaying the Azure portal, in the toolbar, click the **Cloud Shell** icon located directly to the right of the search text box.

3. If prompted to select either **Bash** or **PowerShell**, select **Bash**.

   > **Note**: If this is the first time you are starting **Cloud Shell** and you are presented with the **You have no storage mounted** message, select the subscription you are using in this lab, and select **Create storage**.

4. From the **Bash** session in the Cloud Shell pane, run the following to create variables representing the Azure region where you will deploy resources in this lab, the resource group containing these resources, as well as the names of these resources, including an Azure Container Registry instance, an Azure App Service plan name, an Azure web app name, an Azure SQL Database logical server name, and an Azure SQL database name:

5. From the Bash session in the Cloud Shell pane, run the following to create the resource group that will host Azure resources you deploy in this lab (replace the `<Azure_region>` placeholder with the name of the Azure region, such as ‘eastus’, where you intend to deploy these resources):

   CodeCopy

   ```bash
   LOCATION=<Azure_region>
   ```

   > **Note**: You can identify Azure region names by running `az account list-locations -o table`

6. Run the following to create variables representing the names of Azure resources, including an Azure Container Registry instance, an Azure App Service plan name, an Azure web app name, an Azure SQL Database logical server name, and an Azure SQL database name:

   CodeCopy

   ```bash
   RG_NAME='az400m1501a-RG'
   ACR_NAME=az400m151acr$RANDOM$RANDOM
   APP_SVC_PLAN='az400m1501a-app-svc-plan'
   WEB_APP_NAME=az400m151web$RANDOM$RANDOM
   SQLDB_SRV_NAME=az400m15sqlsrv$RANDOM$RANDOM
   SQLDB_NAME='az400m15sqldb'
   ```

   > **Note**: You can identify Azure region names by running `az account list-locations -o table`

7. Run the following to create all resources required Azure resources required for this lab:

   CodeCopy

   ```bash
   az group create --name $RG_NAME --location $LOCATION
   az acr create --name $ACR_NAME --resource-group $RG_NAME --location $LOCATION --sku Standard --admin-enabled true
   az appservice plan create --name 'az400m1501a-app-svc-plan' --location $LOCATION --resource-group $RG_NAME --is-linux
   az webapp create --name $WEB_APP_NAME --resource-group $RG_NAME --plan $APP_SVC_PLAN --deployment-container-image-name elnably/dockerimagetest
   IMAGE_NAME=myhealth.web
   az webapp config container set --name $WEB_APP_NAME --resource-group $RG_NAME --docker-custom-image-name $IMAGE_NAME --docker-registry-server-url $ACR_NAME.azurecr.io/$IMAGE_NAME:latest --docker-registry-server-url https://$ACR_NAME.azurecr.io
   az sql server create --name $SQLDB_SRV_NAME --resource-group $RG_NAME --location $LOCATION --admin-user sqladmin --admin-password Pa55w.rd1234
   az sql db create --name $SQLDB_NAME --resource-group $RG_NAME --server $SQLDB_SRV_NAME --service-objective S0 --no-wait 
   az sql server firewall-rule create --name AllowAllAzure --resource-group $RG_NAME --server $SQLDB_SRV_NAME --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
   ```

   > **Note**: Wait for the provisioning process to complete. This might take about 5 minutes.

   ![LAB15-Az400_10](Evidencia/LAB15-Az400_10.png)

   ![LAB15-Az400_11](Evidencia/LAB15-Az400_11.png)

   ![LAB15-Az400_12](Evidencia/LAB15-Az400_12.png)

   ![LAB15-Az400_13](Evidencia/LAB15-Az400_13.png)

   ![LAB15-Az400_14](Evidencia/LAB15-Az400_14.png)

   ![LAB15-Az400_15](Evidencia/LAB15-Az400_15.png)

   ![LAB15-Az400_16](Evidencia/LAB15-Az400_16.png)

   ![LAB15-Az400_17](Evidencia/LAB15-Az400_17.png)

   ![LAB15-Az400_18](Evidencia/LAB15-Az400_18.png)

   ![LAB15-Az400_19](Evidencia/LAB15-Az400_19.png)

   ![LAB15-Az400_20](Evidencia/LAB15-Az400_20.png)

   ![LAB15-Az400_21](Evidencia/LAB15-Az400_21.png)

8. Run the following to configure a connection string of the newly created Azure web app (replace the $SQLDB_SRV_NAME and $SQLDB_NAME placeholders with the values of the names of the Azure SQL Database logical server and its database instance, respectively):

   CodeCopy

   ```bash
   CONNECTION_STRING="Data Source=tcp:$SQLDB_SRV_NAME.database.windows.net,1433;Initial Catalog=$SQLDB_NAME;User Id=sqladmin;Password=Pa55w.rd1234;"
   az webapp config connection-string set --name $WEB_APP_NAME --resource-group $RG_NAME --connection-string-type SQLAzure --settings defaultConnection="$CONNECTION_STRING"
   ```

   ![LAB15-Az400_22](Evidencia/LAB15-Az400_22.png)

   ![LAB15-Az400_23](Evidencia/LAB15-Az400_23.png)

   ![LAB15-Az400_24](Evidencia/LAB15-Az400_24.png)

9. In the web browser displaying the Azure portal, close the Cloud Shell pane, navigate to the **Resource groups** blade, and, on the **Resource groups** blade, select the **az400m1501a-RG** entry.

10. On the **az400m1501a-RG** resource group blade, review the listing of its resources.

    ![LAB15-Az400_25](Evidencia/LAB15-Az400_25.png)

    > **Note**: Record the name of the logical Azure SQL Database server. You will need it later in this lab.

11. On the **az400m1501a-RG** resource group blade, in the list of resources, click the entry representing the Container Registry instance.

    ![LAB15-Az400_26](Evidencia/LAB15-Az400_26.png)

12. On the container registry blade, in the vertical menu on the left side, in the **Settings** section, click **Access keys**.

    ![LAB15-Az400_27](Evidencia/LAB15-Az400_27.png)

13. On the **Access keys** blade of the container registry instance, identify the values of the **Registry name**, **Login server**, **Admin user**, and **password** entries.

    ![LAB15-Az400_28](Evidencia/LAB15-Az400_28.png)

    > **Note**: Record the **Registry name**, **Login server**, and **password** entries (the registry names and the admin user name should match). You will need it later in this lab.

### Exercise 2: Deploy a Docker container to Azure App Service web app by using Azure DevOps

In this exercise, you will deploy a Docker container to Azure App Service web app by using Azure DevOps.

#### Task 1: Configure Continuous Integration (CI) and Continuous Delivery (CD)

In this task, you will use the Azure DevOps project you generated in the previous exercise in order to implement a CI/CD pipeline that builds and deploys a Docker container to an Azure App Service web app.

1. On your lab computer, switch to the web browser window displaying the Azure DevOps portal with the **Deploying Docker containers to Azure App Service web apps** project open, in the vertical menu bar at the far left of the Azure DevOps portal, click **Pipelines**.

   > **Note**: You will first modify the build pipeline.

2. On the **Pipelines** pane, click the entry representing the **MHCDocker.build** pipeline and, on the **MHCDocker.build** pane, click **Edit**.

   ![LAB15-Az400_29](Evidencia/LAB15-Az400_29.png)

   ![LAB15-Az400_30](Evidencia/LAB15-Az400_30.png)

   > **Note**: The build pipeline consists of the following tasks

   | Tasks                | Usage                                                        |
   | :------------------- | :----------------------------------------------------------- |
   | **Run services**     | prepares the build environment by restoring the required packages |
   | **Build services**   | builds the **myhealth.web** image                            |
   | **Push services**    | pushes the **myhealth.web** image tagged with **$(Build.BuildId)** to container registry |
   | **Publish Artifact** | allows for sharing dacpac for database deployment through Azure DevOps artifacts |

   ![LAB15-Az400_31](Evidencia/LAB15-Az400_31.png)

3. On the **MHCDocker.build** pipeline pane, in the list of tasks of the pipeline, click the **Run services** task, on the **Docker Compose** pane on the right side, in the **Azure subscription** dropdown list, select the entry representing the Azure subscription you are using in this lab, and click **Authorize** to create the corresponding service connection. When prompted, sign in using the account with the Owner role in the Azure subscription and the Global Administrator role in the Azure AD tenant associated with the Azure subscription.

   > **Note**: This step creates an Azure service connection, which defines and secures a connection to the target Azure subscription, using Service Principal Authentication (SPA).

   ![LAB15-Az400_32](Evidencia/LAB15-Az400_32.png)

   ![LAB15-Az400_33](Evidencia/LAB15-Az400_33.png)

   ![LAB15-Az400_34](Evidencia/LAB15-Az400_34.png)

   

4. In the list of tasks of the pipeline, with the **Run services** task selected, on the **Docker Compose** pane on the right side, in the **Azure Container Registry** dropdown list, select the entry representing the ACR instance you created earlier in this lab (**refresh the list if needed** or type the name of the login server).

   ![LAB15-Az400_35](Evidencia/LAB15-Az400_35.png)

   

5. Repeat the previous two steps to configure the **Azure subscription** and **Azure Container Registry** settings in the **Build services**, and **Push services** tasks, but, this time, instead of selecting your Azure subscription, select the newly created service connection.

   ![LAB15-Az400_36](Evidencia/LAB15-Az400_36.png)

   ![LAB15-Az400_37](Evidencia/LAB15-Az400_37.png)

   ![LAB15-Az400_38](Evidencia/LAB15-Az400_38.png)

   ![LAB15-Az400_39](Evidencia/LAB15-Az400_39.png)

6. On the same **MHCDocker.build** pipeline pane, at the top of the pane, click the down-facing caret next to the **Save & queue** button, click **Save** to save the changes, and, when prompted again, click **Save**.

   ![LAB15-Az400_40](Evidencia/LAB15-Az400_40.png)

   ![LAB15-Az400_41](Evidencia/LAB15-Az400_41.png)

   > **Note**: Next you will modify the release pipeline.

7. In the web browser window displaying the Azure DevOps portal, in the vertical menu bar at the far left of the Azure DevOps portal, in the **Pipelines** section, click **Releases**.

8. On the **Pipelines / Releases** pane, ensure that the **MHCDocker.release** entry is selected and click **Edit**.

   ![LAB15-Az400_42](Evidencia/LAB15-Az400_42.png)

9. On the **All pipelines / MHCDocker.release** pane, in the rectangle representing the **Dev** stage of the deployment, click the **2 jobs, 2 tasks** link.

   ![LAB15-Az400_43](Evidencia/LAB15-Az400_43.png)

   > **Note**: The release pipeline consists of the following tasks

   | Tasks                             | Usage                                                        |
   | :-------------------------------- | :----------------------------------------------------------- |
   | **Execute Azure SQL: DacpacTask** | deploys the dacpac artifact including the target schema and data to the Azure SQL database |
   | **Azure App Service deploy**      | pulls the docker image generated during the build stage from the designated container registry and deploys the image to the Azure App Service web app |

   ![LAB15-Az400_44](Evidencia/LAB15-Az400_44.png)

10. In the list of tasks of the pipeline, click the **Execute Azure SQL: DacpacTask** task, on the **Azure SQL Database deployment** pane on the right side, in the **Azure subscription** dropdown list, select the entry representing the Azure service connection you created earlier in this task.

    ![LAB15-Az400_45](Evidencia/LAB15-Az400_45.png)

    ![LAB15-Az400_46](Evidencia/LAB15-Az400_46.png)

11. In the list of tasks of the pipeline, click the **Azure App Service deploy** task, on the **Azure App Service deploy** pane on the right side, in the **Azure subscription** dropdown list, select the entry representing the Azure service connection you created earlier in this task and, in the **App Service name** dropdown list, select the entry representing the Azure App Service web app you deployed earlier in this lab.

    ![LAB15-Az400_47](Evidencia/LAB15-Az400_47.png)

    ![LAB15-Az400_48](Evidencia/LAB15-Az400_48.png)

    > **Note**: Next you will need to configure the agent pool information required for deployment.

12. Select the **DB deployment** job, on the **Agent job** pane on the right side, in the **Agent pool** dropdown list, select **Azure Pipelines** and, next, in the **Agent Specification** dropdown list, select **vs2017-win2016**.

    ![LAB15-Az400_49](Evidencia/LAB15-Az400_49.png)

    ![LAB15-Az400_50](Evidencia/LAB15-Az400_50.png)

13. Select the **Web App deployment** job, on the **Agent job** pane on the right side, in the **Agent pool** dropdown list, select **Azure Pipelines** and, next, in the **Agent Specification** dropdown list, select **ubuntu-16.04**.

    ![LAB15-Az400_51](Evidencia/LAB15-Az400_51.png)

    ![LAB15-Az400_52](Evidencia/LAB15-Az400_52.png)

    ![LAB15-Az400_53](Evidencia/LAB15-Az400_53.png)

14. At the top of the pane, click the **Variables** header.

    ![LAB15-Az400_54](Evidencia/LAB15-Az400_54.png)

15. In the list of pipeline variables, set the values of the following variables:

    | Variable     | Value                                                        |
    | :----------- | :----------------------------------------------------------- |
    | ACR          | Azure container registry login name you recorded in the previous exercise of this lab, including the **azurecr.io** suffix |
    | DatabaseName | **az400m15sqldb**                                            |
    | Password     | **Pa55w.rd1234**                                             |
    | SQLadmin     | **sqladmin**                                                 |
    | SQLserver    | the name of the Azure SQL Database logical server you recorded in the previous exercise of this lab, including the **database.windows.net** suffix |

    ![LAB15-Az400_55](Evidencia/LAB15-Az400_55.png)

    ![LAB15-Az400_56](Evidencia/LAB15-Az400_56.png)

    ![LAB15-Az400_57](Evidencia/LAB15-Az400_57.png)

    ![LAB15-Az400_58](Evidencia/LAB15-Az400_58.png)

    ![LAB15-Az400_59](Evidencia/LAB15-Az400_59.png)

    

16. In the upper right corner of the pane, click the **Save** button to save the changes, and, when prompted again, click **Save**.

    ![LAB15-Az400_60](Evidencia/LAB15-Az400_60.png)

    ![LAB15-Az400_61](Evidencia/LAB15-Az400_61.png)

#### Task 2: Trigger build and release pipelines by using code commit

In this exercise, you will trigger the build and release pipelines by using code commit.

1. In the web browser window displaying the Azure DevOps portal, in the vertical menu bar at the far left of the Azure DevOps portal, click **Repos**.

   ![LAB15-Az400_62](Evidencia/LAB15-Az400_62.png)

   > **Note**: This will automatically display the **Files** pane.

2. On the **Files** pane, navigate to the **src/MyHealth.Web/Views/Home** folder and click the entry representing the **Index.cshtml** file and then click **Edit** to open it for editing.

   ![LAB15-Az400_63](Evidencia/LAB15-Az400_63.png)

   ![LAB15-Az400_64](Evidencia/LAB15-Az400_64.png)

3. On the **Index.cshtml** pane, in line **28**, change **JOIN US** to **CONTACT US** and then, in the upper right corner of the pane, click **Commit** and, when prompted for confirmation, click **Commit** again.

   ![LAB15-Az400_65](Evidencia/LAB15-Az400_65.png)

   ![LAB15-Az400_66](Evidencia/LAB15-Az400_66.png)

   ![LAB15-Az400_67](Evidencia/LAB15-Az400_67.png)

   > **Note**: This action would initiate an automatic build for the source code.

4. In the web browser window displaying the Azure DevOps portal, in the vertical menu bar at the far left of the Azure DevOps portal, click **Pipelines**.

5. On the **Pipelines** pane, click the entry representing the pipeline run triggered by the commit.

   ![LAB15-Az400_68](Evidencia/LAB15-Az400_68.png)

6. On the **MHCDocker.build** pane, click the entry representing the pipeline run.

   ![LAB15-Az400_69](Evidencia/LAB15-Az400_69.png)

7. On the **Summary** tab of the pipeline run, in the **Jobs** section, click the **Docker** entry and, on the resulting pane, monitor the progress of individual tasks until the job successfully completes.

   ![LAB15-Az400_70](Evidencia/LAB15-Az400_70.png)

   ![LAB15-Az400_71](Evidencia/LAB15-Az400_71.png)

   ![LAB15-Az400_72](Evidencia/LAB15-Az400_72.png)

   ![LAB15-Az400_73](Evidencia/LAB15-Az400_73.png)

   ![LAB15-Az400_74](Evidencia/LAB15-Az400_74.png)

   ![LAB15-Az400_75](Evidencia/LAB15-Az400_75.png)

   ![LAB15-Az400_76](Evidencia/LAB15-Az400_76.png)

   ![LAB15-Az400_77](Evidencia/LAB15-Az400_77.png)

   ![LAB15-Az400_78](Evidencia/LAB15-Az400_78.png)

   ![LAB15-Az400_79](Evidencia/LAB15-Az400_79.png)

   ![LAB15-Az400_80](Evidencia/LAB15-Az400_80.png)

   > **Note**: The build generates a Docker image and pushes it to Azure Container Registry. Once the build completes, you will be able to review its summary.

8. In the web browser window displaying the Azure DevOps portal, in the vertical menu bar at the far left of the Azure DevOps portal, in the **Pipelines** section, click **Releases**.

9. On the **Releases** pane, click the entry representing the latest release triggered by the successful build.

   ![LAB15-Az400_81](Evidencia/LAB15-Az400_81.png)

10. On the **MHCDocker.release > Release-1** pane, select the rectangle representing the **Dev** stage.

    ![LAB15-Az400_82](Evidencia/LAB15-Az400_82.png)

11. On the **MHCDocker.release > Release-1 > Dev** pane, monitor the progress of the release tasks until their successful completion.

    ![LAB15-Az400_83](Evidencia/LAB15-Az400_83.png)

    > **Note**: The release deploys the docker image generated by the build process to the App Service web app. Once the release completes, you can review its summary and logs.

    ![LAB15-Az400_84](Evidencia/LAB15-Az400_84.png)

    ![LAB15-Az400_85](Evidencia/LAB15-Az400_85.png)

    ![LAB15-Az400_86](Evidencia/LAB15-Az400_86.png)

    ![LAB15-Az400_87](Evidencia/LAB15-Az400_87.png)

    ![LAB15-Az400_88](Evidencia/LAB15-Az400_88.png)

    ![LAB15-Az400_89](Evidencia/LAB15-Az400_89.png)

    ![LAB15-Az400_90](Evidencia/LAB15-Az400_90.png)

    ![LAB15-Az400_91](Evidencia/LAB15-Az400_91.png)

    ![LAB15-Az400_92](Evidencia/LAB15-Az400_92.png)

    ![LAB15-Az400_93](Evidencia/LAB15-Az400_93.png)

    ![LAB15-Az400_94](Evidencia/LAB15-Az400_94.png)

    ![LAB15-Az400_95](Evidencia/LAB15-Az400_95.png)

    ![LAB15-Az400_96](Evidencia/LAB15-Az400_96.png)

12. After the release pipeline completes, switch to the web browser window displaying the [Azure Portal](https://portal.azure.com/) and navigate to the blade of the Azure App Service web app you provisioned earlier in this lab.

    ![LAB15-Az400_97](Evidencia/LAB15-Az400_97.png)

13. On the App Service web app, click the **URL** link entry representing the target web app.

    ![LAB15-Az400_98](Evidencia/LAB15-Az400_98.png)

    ![LAB15-Az400_99](Evidencia/LAB15-Az400_99.png)

    ![LAB15-Az400_100](Evidencia/LAB15-Az400_100.png)

    ![LAB15-Az400_101](Evidencia/LAB15-Az400_101.png)

    ![LAB15-Az400_102](Evidencia/LAB15-Az400_102.png)

    ![LAB15-Az400_103](Evidencia/LAB15-Az400_103.png)

    ![LAB15-Az400_104](Evidencia/LAB15-Az400_104.png)

    ![LAB15-Az400_105](Evidencia/LAB15-Az400_105.png)

    ![LAB15-Az400_106](Evidencia/LAB15-Az400_106.png)

    ![LAB15-Az400_107](Evidencia/LAB15-Az400_107.png)

    ![LAB15-Az400_108](Evidencia/LAB15-Az400_108.png)

    ![LAB15-Az400_109](Evidencia/LAB15-Az400_109.png)

    ![LAB15-Az400_110](Evidencia/LAB15-Az400_110.png)

    ![LAB15-Az400_111](Evidencia/LAB15-Az400_111.png)

    ![LAB15-Az400_112](Evidencia/LAB15-Az400_112.png)

    ![LAB15-Az400_113](Evidencia/LAB15-Az400_113.png)

    ![LAB15-Az400_114](Evidencia/LAB15-Az400_114.png)

    ![LAB15-Az400_115](Evidencia/LAB15-Az400_115.png)

    ![LAB15-Az400_116](Evidencia/LAB15-Az400_116.png)

    ![LAB15-Az400_117](Evidencia/LAB15-Az400_117.png)

    ![LAB15-Az400_118](Evidencia/LAB15-Az400_118.png)

    ![LAB15-Az400_119](Evidencia/LAB15-Az400_119.png)

    ![LAB15-Az400_120](Evidencia/LAB15-Az400_120.png)

    ![LAB15-Az400_121](Evidencia/LAB15-Az400_121.png)

    ![LAB15-Az400_122](Evidencia/LAB15-Az400_122.png)

    ![LAB15-Az400_123](Evidencia/LAB15-Az400_123.png)

    ![LAB15-Az400_124](Evidencia/LAB15-Az400_124.png)

    ![LAB15-Az400_125](Evidencia/LAB15-Az400_125.png)

    ![LAB15-Az400_126](Evidencia/LAB15-Az400_126.png)

    ![LAB15-Az400_127](Evidencia/LAB15-Az400_127.png)

    ![LAB15-Az400_128](Evidencia/LAB15-Az400_128.png)

    ![LAB15-Az400_129](Evidencia/LAB15-Az400_129.png)

    ![LAB15-Az400_130](Evidencia/LAB15-Az400_130.png)

    ![LAB15-Az400_131](Evidencia/LAB15-Az400_131.png)

    ![LAB15-Az400_132](Evidencia/LAB15-Az400_132.png)

    ![LAB15-Az400_133](Evidencia/LAB15-Az400_133.png)

    ![LAB15-Az400_134](Evidencia/LAB15-Az400_134.png)

    ![LAB15-Az400_135](Evidencia/LAB15-Az400_135.png)

    ![LAB15-Az400_136](Evidencia/LAB15-Az400_136.png)

    ![LAB15-Az400_137](Evidencia/LAB15-Az400_137.png)

    ![LAB15-Az400_138](Evidencia/LAB15-Az400_138.png)

    ![LAB15-Az400_139](Evidencia/LAB15-Az400_139.png)

    ![LAB15-Az400_140](Evidencia/LAB15-Az400_140.png)

    ![LAB15-Az400_141](Evidencia/LAB15-Az400_141.png)

    ![LAB15-Az400_142](Evidencia/LAB15-Az400_142.png)

    ![LAB15-Az400_143](Evidencia/LAB15-Az400_143.png)

    ![LAB15-Az400_144](Evidencia/LAB15-Az400_144.png)

    ![LAB15-Az400_145](Evidencia/LAB15-Az400_145.png)

    ![LAB15-Az400_146](Evidencia/LAB15-Az400_146.png)

    ![LAB15-Az400_147](Evidencia/LAB15-Az400_147.png)

    ![LAB15-Az400_148](Evidencia/LAB15-Az400_148.png)

    ![LAB15-Az400_149](Evidencia/LAB15-Az400_149.png)

    ![LAB15-Az400_150](Evidencia/LAB15-Az400_150.png)

    ![LAB15-Az400_151](Evidencia/LAB15-Az400_151.png)

    ![LAB15-Az400_152](Evidencia/LAB15-Az400_152.png)

    ![LAB15-Az400_153](Evidencia/LAB15-Az400_153.png)

    ![LAB15-Az400_154](Evidencia/LAB15-Az400_154.png)

    ![LAB15-Az400_155](Evidencia/LAB15-Az400_155.png)

    ![LAB15-Az400_156](Evidencia/LAB15-Az400_156.png)

    ![LAB15-Az400_157](Evidencia/LAB15-Az400_157.png)

    ![LAB15-Az400_158](Evidencia/LAB15-Az400_158.png)

    ![LAB15-Az400_159](Evidencia/LAB15-Az400_159.png)

    ![LAB15-Az400_160](Evidencia/LAB15-Az400_160.png)

    ![LAB15-Az400_161](Evidencia/LAB15-Az400_161.png)

    ![LAB15-Az400_162](Evidencia/LAB15-Az400_162.png)

    ![LAB15-Az400_163](Evidencia/LAB15-Az400_163.png)

    ![LAB15-Az400_164](Evidencia/LAB15-Az400_164.png)

    ![LAB15-Az400_165](Evidencia/LAB15-Az400_165.png)

    ![LAB15-Az400_166](Evidencia/LAB15-Az400_166.png)

    ![LAB15-Az400_167](Evidencia/LAB15-Az400_167.png)

    ![LAB15-Az400_168](Evidencia/LAB15-Az400_168.png)

    ![LAB15-Az400_169](Evidencia/LAB15-Az400_169.png)

    ![LAB15-Az400_170](Evidencia/LAB15-Az400_170.png)

    ![LAB15-Az400_171](Evidencia/LAB15-Az400_171.png)

    ![LAB15-Az400_172](Evidencia/LAB15-Az400_172.png)

    ![LAB15-Az400_173](Evidencia/LAB15-Az400_173.png)

    ![LAB15-Az400_174](Evidencia/LAB15-Az400_174.png)

    ![LAB15-Az400_175](Evidencia/LAB15-Az400_175.png)

    ![LAB15-Az400_176](Evidencia/LAB15-Az400_176.png)

    ![LAB15-Az400_177](Evidencia/LAB15-Az400_177.png)

    ![LAB15-Az400_178](Evidencia/LAB15-Az400_178.png)

    ![LAB15-Az400_179](Evidencia/LAB15-Az400_179.png)

    ![LAB15-Az400_180](Evidencia/LAB15-Az400_180.png)

    ![LAB15-Az400_181](Evidencia/LAB15-Az400_181.png)

    ![LAB15-Az400_182](Evidencia/LAB15-Az400_182.png)

    ![LAB15-Az400_183](Evidencia/LAB15-Az400_183.png)

    ![LAB15-Az400_184](Evidencia/LAB15-Az400_184.png)

    ![LAB15-Az400_185](Evidencia/LAB15-Az400_185.png)

    ![LAB15-Az400_186](Evidencia/LAB15-Az400_186.png)

    ![LAB15-Az400_187](Evidencia/LAB15-Az400_187.png)

    ![LAB15-Az400_188](Evidencia/LAB15-Az400_188.png)

    ![LAB15-Az400_189](Evidencia/LAB15-Az400_189.png)

    ![LAB15-Az400_190](Evidencia/LAB15-Az400_190.png)

    ![LAB15-Az400_191](Evidencia/LAB15-Az400_191.png)

    ![LAB15-Az400_192](Evidencia/LAB15-Az400_192.png)

    ![LAB15-Az400_193](Evidencia/LAB15-Az400_193.png)

    ![LAB15-Az400_194](Evidencia/LAB15-Az400_194.png)

    ![LAB15-Az400_195](Evidencia/LAB15-Az400_195.png)

    > **Note**: This will automatically open a new web browser tab displaying the target web site.

14. Verify that the target web app displays the HealthClinic.biz web site, including the change that you applied to trigger the CI/CD pipeline.

### Exercise 3: Remove the Azure lab resources

In this exercise, you will remove the Azure resources provisioned in this lab to eliminate unexpected charges.

> **Note**: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges.

#### Task 1: Remove the Azure lab resources

In this task, you will use Azure Cloud Shell to remove the Azure resources provisioned in this lab to eliminate unnecessary charges.

1. In the Azure portal, open the **Bash** shell session within the **Cloud Shell** pane.

2. List all resource groups created throughout the labs of this module by running the following command:

   ShellCopy

   ```sh
   az group list --query "[?starts_with(name,'az400m1501')].name" --output tsv
   ```

   ![LAB15-Az400_196](Evidencia/LAB15-Az400_196.png)

3. Delete all resource groups you created throughout the labs of this module by running the following command:

   ShellCopy

   ```sh
   az group list --query "[?starts_with(name,'az400m1501')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

   > **Note**: The command executes asynchronously (as determined by the –nowait parameter), so while you will be able to run another Azure CLI command immediately afterwards within the same Bash session, it will take a few minutes before the resource groups are actually removed.

![LAB15-Az400_197](Evidencia/LAB15-Az400_197.png)

![LAB15-Az400_198](Evidencia/LAB15-Az400_198.png)

![LAB15-Az400_199](Evidencia/LAB15-Az400_199.png)

## Review

In this lab, you used an Azure DevOps CI/CD pipeline to build a custom Docker image, pushed it to Azure Container Registry, and deployed it as a container to Azure App Service by using Azure DevOps.