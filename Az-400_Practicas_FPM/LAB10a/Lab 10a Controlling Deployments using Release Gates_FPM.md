# Lab 10a: Controlling Deployments using Release Gates

# Student lab manual

## Lab overview

This lab covers the configuration of the deployment gates and details how to use them to control execution of Azure pipelines. To illustrate their implementation, you will configure a release definition with two environments for an Azure Web App. You will deploy to the Canary environment only when there are no blocking bugs for the app and mark the Canary environment complete only when there are no active alerts in Application Insights of Azure Monitor.

A release pipeline specifies the end-to-end release process for an application to be deployed across a range of environments. Deployments to each environment are fully automated by using jobs and tasks. Ideally, you do not want new updates to the applications to be exposed to all the users at the same time. It is a best practice to expose updates in a phased manner i.e. expose to a subset of users, monitor their usage and expose to other users based on the experience of the initial set of users.

Approvals and gates enable you to take control over the start and completion of the deployments in a release. With approvals, you can wait for users to manually approve or reject deployments. Using release gates, you can specify application health criteria that must be met before release is promoted to the next environment. Prior to or after any environment deployment, all the specified gates are automatically evaluated until they all pass or until they reach your defined timeout period and fail.

Gates can be added to an environment in the release definition from the pre-deployment conditions or the post-deployment conditions panel. Multiple gates can be added to the environment conditions to ensure all the inputs are successful for the release.

As an example:

- Pre-deployment gates ensure there are no active issues in the work item or problem management system before deploying a build to an environment.
- Post-deployment gates ensure there are no incidents from the monitoring or incident management system for the app after it’s been deployed, before promoting the release to the next environment.

There are 4 types of gates included by default in every account.

- Invoke Azure Function: Triggers execution of an Azure Function and ensures a successful completion.
- Query Azure Monitor alerts: Observes the configured Azure Monitor alert rules for active alerts.
- Invoke REST API: Makes a call to a REST API and continues if it returns a successful response.
- Query work items: Ensures the number of matching work items returned from a query is within a threshold.

## Objectives

After you complete this lab, you will be able to:

- Configure release pipelines
- Configure release gates
- Test release gates

## Lab duration

- Estimated time: **75 minutes**

## Instructions

### Before you start

#### Sign in to the lab virtual machine

Ensure that you’re signed in to your Windows 10 virtual machine by using the following credentials:

- Username: **Student**
- Password: **Pa55w.rd**

#### Review applications required for this lab

Identify the applications that you’ll use in this lab:

- Microsoft Edge

#### Set up an Azure DevOps organization.

If you don’t already have an Azure DevOps organization that you can use for this lab, create one by following the instructions available at [Create an organization or project collection](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/create-organization?view=azure-devops).

#### Prepare an Azure subscription

- Identify an existing Azure subscription or create a new one.
- Verify that you have a Microsoft account or an Azure AD account with the Owner role in the Azure subscription and the Global Administrator role in the Azure AD tenant associated with the Azure subscription. For details, refer to [List Azure role assignments using the Azure portal](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-list-portal) and [View and assign administrator roles in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/roles/manage-roles-portal#view-my-roles).

### Exercise 0: Configure the lab prerequisites

In this exercise, you will set up the prerequisites for the lab, which include the preconfigured Parts Unlimited team project based on an Azure DevOps Demo Generator template and two Azure web apps representing the **Canary** and **Production** environments, into which you’ll deploy the application via Azure Pipelines.

#### Task 1: Configure the team project

In this task, you will use Azure DevOps Demo Generator to generate a new project based on the **ReleaseGates** template.

1. On your lab computer, start a web browser and navigate to [Azure DevOps Demo Generator](https://azuredevopsdemogenerator.azurewebsites.net/). This utility site will automate the process of creating a new Azure DevOps project within your account that is prepopulated with content (work items, repos, etc.) required for the lab.

   > **Note**: For more information on the site, see https://docs.microsoft.com/en-us/azure/devops/demo-gen.

2. Click **Sign in** and sign in using the Microsoft account associated with your Azure DevOps subscription.

   ![LAB10a-Az400_01](Evidencia/LAB10a-Az400_01.png)

3. If required, on the **Azure DevOps Demo Generator** page, click **Accept** to accept the permission requests for accessing your Azure DevOps subscription.

4. On the **Create New Project** page, in the **New Project Name** textbox, type **Controlling Deployments using Release Gates**, in the **Select organization** dropdown list, select your Azure DevOps organization, and then click **Choose template**.

   ![LAB10a-Az400_02](Evidencia/LAB10a-Az400_02.png)

5. In the list of templates, in the toolbar, click **DevOps Labs**, select the **ReleaseGates** template and click **Select Template**.

   ![LAB10a-Az400_03](Evidencia/LAB10a-Az400_03.png)

6. Back on the **Create New Project** page, click **Create Project**

   ![LAB10a-Az400_04](Evidencia/LAB10a-Az400_04.png)

   ![LAB10a-Az400_05](Evidencia/LAB10a-Az400_05.png)

   > **Note**: Wait for the process to complete. This should take about 2 minutes. In case the process fails, navigate to your DevOps organization, delete the project, and try again.

   ![LAB10a-Az400_06](Evidencia/LAB10a-Az400_06.png)

7. On the **Create New Project** page, click **Navigate to project**.

   ![LAB10a-Az400_07](Evidencia/LAB10a-Az400_07.png)

   ![LAB10a-Az400_08](Evidencia/LAB10a-Az400_08.png)

   ![LAB10a-Az400_09](Evidencia/LAB10a-Az400_09.png)

#### Task 2: Create two Azure web apps

In this task, you will create two Azure web apps representing the **Canary** and **Production** environments, into which you’ll deploy the application via Azure Pipelines.

1. From the lab computer, start a web browser, navigate to the [**Azure Portal**](https://portal.azure.com/), and sign in with the user account that has the Owner role in the Azure subscription you will be using in this lab and has the role of the Global Administrator in the Azure AD tenant associated with this subscription.

   ![LAB10a-Az400_10](Evidencia/LAB10a-Az400_10.png)

   ![LAB10a-Az400_11](Evidencia/LAB10a-Az400_11.png)

   ![LAB10a-Az400_12](Evidencia/LAB10a-Az400_12.png)

2. In the Azure portal, click the **Cloud Shell** icon, located directly to the right of the search textbox at the top of the page.

3. If prompted to select either **Bash** or **PowerShell**, select **Bash**.

   > **Note**: If this is the first time you are starting **Cloud Shell** and you are presented with the **You have no storage mounted** message, select the subscription you are using in this lab, and select **Create storage**.

4. From the **Bash** prompt, in the **Cloud Shell** pane, run the following command to create a resource group (replace the `<region>` placeholder with the name of the Azure region that will host the two Azure web apps, for example ‘westeurope’ or ‘eastus’):

   > **Note**: possible locations can be found by running the following command, use the **Name** on `<region>` : `az account list-locations -o table`

   ![LAB10a-Az400_13](Evidencia/LAB10a-Az400_13.png)

   ![LAB10a-Az400_14](Evidencia/LAB10a-Az400_14.png)

   CodeCopy

   ```bash
   RESOURCEGROUPNAME='az400m10l01-RG'
   az group create -n $RESOURCEGROUPNAME -l '<region>'
   ```

   ![LAB10a-Az400_15](Evidencia/LAB10a-Az400_15.png)

5. To create an App service plan

   CodeCopy

   ```bash
   SERVICEPLANNAME='az400m01l01-sp1'
   az appservice plan create -g $RESOURCEGROUPNAME -n $SERVICEPLANNAME --sku S1
   ```

   ![LAB10a-Az400_16](Evidencia/LAB10a-Az400_16.png)

6. Create two web apps with unique app names.

   CodeCopy

   ```bash
    SUFFIX=$RANDOM$RANDOM
    az webapp create -g $RESOURCEGROUPNAME -p $SERVICEPLANNAME -n PU$SUFFIX-Canary
    az webapp create -g $RESOURCEGROUPNAME -p $SERVICEPLANNAME -n PU$SUFFIX-Prod
   ```

   ![LAB10a-Az400_17](Evidencia/LAB10a-Az400_17.png)

   ![LAB10a-Az400_18](Evidencia/LAB10a-Az400_18.png)

   ![LAB10a-Az400_19](Evidencia/LAB10a-Az400_19.png)

7. Navigate to the resource group **az400m10l01-RG** you created earlier in this task and review the resources you created.

   ![LAB10a-Az400_20](Evidencia/LAB10a-Az400_20.png)

8. In the list of resources, click the **Canary** web app.

   ![LAB10a-Az400_21](Evidencia/LAB10a-Az400_21.png)

9. On the **Canary** web app page, in the vertical menu on the left, in the **Settings** section, click **Application Insights**.

10. On the **Application Insights** blade, click **Turn on Application Insights**, accept the default settings, and click **Apply** to create and connect Application Insights resource to your Canary web app. When prompted for confirmation, click **Yes**.

    ![LAB10a-Az400_22](Evidencia/LAB10a-Az400_22.png)

    

11. Wait until the Application Insights resource is created and **click on the name** from the showed “Your app is connected to Application Insights resource: **NAME**” message .

    ![LAB10a-Az400_23](Evidencia/LAB10a-Az400_23.png)

    ![LAB10a-Az400_24](Evidencia/LAB10a-Az400_24.png)

    ![LAB10a-Az400_25](Evidencia/LAB10a-Az400_25.png)

    ![LAB10a-Az400_26](Evidencia/LAB10a-Az400_26.png)

    ![LAB10a-Az400_27](Evidencia/LAB10a-Az400_27.png)

    > **Note**: You will create monitor alerts here, which you will use in later part of this lab.

12. On the **Application Insights resource** window , click **Alerts** option (under Monitoring) and then click **+ New alert rule**.

    ![LAB10a-Az400_28](Evidencia/LAB10a-Az400_28.png)

    ![LAB10a-Az400_29](Evidencia/LAB10a-Az400_29.png)

    ![LAB10a-Az400_30](Evidencia/LAB10a-Az400_30.png)

    ![LAB10a-Az400_31](Evidencia/LAB10a-Az400_31.png)

    ![LAB10a-Az400_32](Evidencia/LAB10a-Az400_32.png)

13. On the **Create alert rule** blade, in the **Condition** section, click the **Add condition** link.

    ![LAB10a-Az400_33](Evidencia/LAB10a-Az400_33.png)

14. On the **Configure signal logic** blade, in the **Search by signal name** textbox, type **Failed Requests** and select it.

    ![LAB10a-Az400_34](Evidencia/LAB10a-Az400_34.png)

15. On the **Configure signal logic** blade, in the **Alert logic** section, leave the **Threshold** set to **Static**, in the **Threshold value** textbox, type **0**, and click on **Done**.

    ![LAB10a-Az400_35](Evidencia/LAB10a-Az400_35.png)

    > **Note**: The rule will generate an alert whenever the number of failed requests is greater than 0 within the last 5 minutes.

    ![LAB10a-Az400_36](Evidencia/LAB10a-Az400_36.png)

16. Back on the **Create alert rule** pane, in the **Alert rule details** specify the following settings and click **Create alert rule** (leave others with their default values):

    | Setting         | Value                             |
    | :-------------- | :-------------------------------- |
    | Alert rule name | **PartsUnlimited_FailedRequests** |
    | Severity        | **Sev 3**                         |

    > **Note**: Metric alert rules might take up to 10 minutes to activate.

    > **Note**: You can create multiple alert rules on different metrics such as availability < 99 percent, server response time > 5 Seconds, or server exceptions > 0

![LAB10a-Az400_37](Evidencia/LAB10a-Az400_37.png)

![LAB10a-Az400_38](Evidencia/LAB10a-Az400_38.png)

### Exercise 1: Configure release pipeline

In this exercise, you will configure a release pipeline.

#### Task 1: Update release tasks

In this task, you will update release tasks.

1. On the lab computer, switch to the browser window displaying the **Controlling Deployments using Release Gates** project in the Azure DevOps portal, in the vertical navigational pane, select **Pipelines** and then, within the **Pipelines** section, click **Releases**.

2. Within the **Releases** view, on the **PartsUnlimited-CD** pane, click **Edit**.

   ![LAB10a-Az400_39](Evidencia/LAB10a-Az400_39.png)

   > **Note**: The pipeline contains two stages named **Canary Environment** and **Production**.

3. On the **Pipeline** tab, in the **Artifacts** rectangle, click the **Continuous deployment trigger** button in the top right corner of the **PartsUnlimited-CI** build artifact.

   ![LAB10a-Az400_40](Evidencia/LAB10a-Az400_40.png)

4. If the continuous deployment trigger for the **PartsUnlimited-CI** build is disabled, toggle the switch to enable it. Leave all other settings at default and close the **Continuous deployment trigger** pane, by clicking the **x** mark in its upper right corner.

   ![LAB10a-Az400_41](Evidencia/LAB10a-Az400_41.png)

5. Within the **Canary Environments** stage, click the **1 job, 2 tasks** label.

   ![LAB10a-Az400_42](Evidencia/LAB10a-Az400_42.png)

   > **Note**: The canary environment has 2 tasks which, respectively, publish the package to Azure Web App and enable continuous monitoring of the application after deployment.

6. On the **All pipelines > PartsUnlimited-CD** pane, ensure that the **Canary Environment** stage is selected. In the **Azure subscription** dropdown list, select your Azure subscription and click **Authorize**. If prompted, authenticate by using the user account with the Owner role in the Azure subscription.

   ![LAB10a-Az400_43](Evidencia/LAB10a-Az400_43.png)

   ![LAB10a-Az400_44](Evidencia/LAB10a-Az400_44.png)

7. In the **App Service name** dropdown list, select the name of the **Canary** web app.

8. In the **Resource Group and Application Insights** dropdown list, select the **az400m10l01-RG** entry.

9. In the **Application Insights resource name** dropdown list, select the name of the **Canary** Application Insights resource, which should match the name of the **Canary** web app.

   ![LAB10a-Az400_45](Evidencia/LAB10a-Az400_45.png)

10. On the **All pipelines > PartsUnlimited-CD** pane, click the **Tasks** tab and, in the dropdown list, select **Production**.

    ![LAB10a-Az400_46](Evidencia/LAB10a-Az400_46.png)

11. With the **Production** stage selected, in the **Azure subscription** dropdown list, select the Azure subscription you used for the **Canary Environment** stage, shown under **Available Azure Service connections**, as we already created the service connection before when authorizing the subscription use.

    ![LAB10a-Az400_47](Evidencia/LAB10a-Az400_47.png)

12. In the **App Service name** dropdown list, select the name of the **Prod** web app.

    ![LAB10a-Az400_48](Evidencia/LAB10a-Az400_48.png)

13. On the **All pipelines > PartsUnlimited-CD** pane, click **Save** and, in the **Save** dialog box, click **OK**.

    ![LAB10a-Az400_49](Evidencia/LAB10a-Az400_49.png)

    ![LAB10a-Az400_50](Evidencia/LAB10a-Az400_50.png)

14. On the **Pipelines** pane, click the entry representing **PartsUnlimited-CI** build pipeline and then, on the **PartsUnlimited-CI** pane, click on **Run Pipeline**.

    ![LAB10a-Az400_51](Evidencia/LAB10a-Az400_51.png)

    ![LAB10a-Az400_52](Evidencia/LAB10a-Az400_52.png)

15. On the **Run pipeline** pane, accept the default settings and click **Run** to trigger the pipeline. **Wait for the build pipeline to finish**.

    ![LAB10a-Az400_53](Evidencia/LAB10a-Az400_53.png)

    > **Note**: After the build succeeds, the release will be triggered automatically and the application will be deployed to both the environments.

16. In the vertical navigational pane, in the **Pipelines** section, click **Releases** and, on the **PartsUnlimited-CD** pane, click the entry representing the most recent release.

    ![LAB10a-Az400_54](Evidencia/LAB10a-Az400_54.png)

17. On the **PartsUnlimited-CD > Release-1** blade, track the progress of the release and verify that the deployment to both web apps completed successfully.

    ![LAB10a-Az400_55](Evidencia/LAB10a-Az400_55.png)

    ![LAB10a-Az400_56](Evidencia/LAB10a-Az400_56.png)

    ![LAB10a-Az400_57](Evidencia/LAB10a-Az400_57.png)

    ![LAB10a-Az400_58](Evidencia/LAB10a-Az400_58.png)

    ![LAB10a-Az400_59](Evidencia/LAB10a-Az400_59.png)

    ![LAB10a-Az400_60](Evidencia/LAB10a-Az400_60.png)

    ![LAB10a-Az400_61](Evidencia/LAB10a-Az400_61.png)

    ![LAB10a-Az400_62](Evidencia/LAB10a-Az400_62.png)

    ![LAB10a-Az400_63](Evidencia/LAB10a-Az400_63.png)

    ![LAB10a-Az400_64](Evidencia/LAB10a-Az400_64.png)

    ![LAB10a-Az400_65](Evidencia/LAB10a-Az400_65.png)

    **Workaround 1. Enabling Gates with Azure Query Monitor**

    ![LAB10a-Az400_66](Evidencia/LAB10a-Az400_66.png)

    ![LAB10a-Az400_67](Evidencia/LAB10a-Az400_67.png)

    ![LAB10a-Az400_68](Evidencia/LAB10a-Az400_68.png)

    ![LAB10a-Az400_68b](Evidencia/LAB10a-Az400_68b.png)

    ![LAB10a-Az400_69](Evidencia/LAB10a-Az400_69.png)

    ![LAB10a-Az400_70](Evidencia/LAB10a-Az400_70.png)

    ![LAB10a-Az400_71](Evidencia/LAB10a-Az400_71.png)

    ![LAB10a-Az400_72](Evidencia/LAB10a-Az400_72.png)

    ![LAB10a-Az400_73](Evidencia/LAB10a-Az400_73.png)

    ![LAB10a-Az400_77](Evidencia/LAB10a-Az400_77.png)

    ![LAB10a-Az400_78](Evidencia/LAB10a-Az400_78.png)

    **Workaround 2. Disabling Continuous Monitoring.**

    ![LAB10a-Az400_79](Evidencia/LAB10a-Az400_79.png)

    ![LAB10a-Az400_80](Evidencia/LAB10a-Az400_80.png)

    ![LAB10a-Az400_81](Evidencia/LAB10a-Az400_81.png)

    ![LAB10a-Az400_82](Evidencia/LAB10a-Az400_82.png)

    ![LAB10a-Az400_83](Evidencia/LAB10a-Az400_83.png)

    ![LAB10a-Az400_84](Evidencia/LAB10a-Az400_84.png)

    ![LAB10a-Az400_85](Evidencia/LAB10a-Az400_85.png)

    ![LAB10a-Az400_86](Evidencia/LAB10a-Az400_86.png)

    ![LAB10a-Az400_87](Evidencia/LAB10a-Az400_87.png)

    ![LAB10a-Az400_88](Evidencia/LAB10a-Az400_88.png)

    ![LAB10a-Az400_89](Evidencia/LAB10a-Az400_89.png)

18. Switch to the Azure portal interface, navigate to the resource group **az400m10l01-RG**, in the list of resources, click the **Canary** web app, on the web app blade, click **Browse**, and verify that the web page loads successfully in a new web browser tab.

    ![LAB10a-Az400_74](Evidencia/LAB10a-Az400_74.png)

    ![LAB10a-Az400_75](Evidencia/LAB10a-Az400_75.png)

    ![LAB10a-Az400_76](Evidencia/LAB10a-Az400_76.png)

19. Close the web browser tab displaying the **Parts Unlimited** web site.

20. Switch to the Azure portal interface, navigate back to the resource group **az400m10l01-RG**, in the list of resources, click the **Production** web app, on the web app blade, click **Browse**, and verify that the web page loads successfully in a new web browser tab.

    ![LAB10a-Az400_90](Evidencia/LAB10a-Az400_90.png)

    ![LAB10a-Az400_91](Evidencia/LAB10a-Az400_91.png)

    ![LAB10a-Az400_92](Evidencia/LAB10a-Az400_92.png)

    ![LAB10a-Az400_93](Evidencia/LAB10a-Az400_93.png)

    

21. Close the web browser tab displaying the **Parts Unlimited** web site.

    > **Note**: Now you have the application with CI/CD configured. In the next exercise we will set up Gates in the release pipeline.

### Exercise 2: Configure release gates.

In this exercise, you will set up Gates in the release pipeline.

#### Task 1: Configure pre-deployment gates

In this task, you will configure pre-deployment gates.

1. Switch to the web browser window displaying the Azure DevOps portal, in the vertical navigational pane, in the **Pipelines** section, click **Releases** and, on the **PartsUnlimited-CD** pane, click **Edit**.

2. On the **All pipelines > PartsUnlimited-CD** pane, on the left edge of the rectangle representing the **Canary Environment** stage, click the oval shape representing the **Pre-deployment conditions**.

   ![LAB10a-Az400_94](Evidencia/LAB10a-Az400_94.png)

3. On **Pre-deployment conditions** pane, set the **Pre-deployment approvals** slider to **Enabled** and, in the **Approvers** text box, type and select your Azure DevOps account name.

   ![LAB10a-Az400_95](Evidencia/LAB10a-Az400_95.png)

4. On **Pre-deployment conditions** pane, set the **Gates** slider to **Enabled**, click **+ Add**, and, in the pop-up menu, click **Query Work Items**.

   ![LAB10a-Az400_96](Evidencia/LAB10a-Az400_96.png)

5. On **Pre-deployment conditions** pane, in the **Query Work Items** section, in the **Query** dropdown list, select **Bugs** under **Shared Queries**, leave the value of **Upper threshold** set to **0**.

   ![LAB10a-Az400_97](Evidencia/LAB10a-Az400_97.png)

   > **Note**: Based on the value of the **Upper threshold** setting, if this query returns any active bug work Item, the release gate will fail.

6. On the **Pre-deployment conditions** pane, leave the value of the **Delay before evaluation** setting at **5 Minutes**.

   ![LAB10a-Az400_98](Evidencia/LAB10a-Az400_98.png)

   > **Note**: **Delay before evaluation** represents the time before the added gates are evaluated for the first time. If no gates are added, then the deployments wait for the duration before proceeding. To allow gate functions to initialize and stabilize (it may take some time for it to begin returning accurate results), we configure a delay before the results are evaluated and used to determine if the deployment should be approved or rejected.

7. On the **Pre-deployment conditions** pane, expand the **Evaluation options** section and configure the following options:

   - Set the value of **Time between re-evaluation of gates** to **5 Minutes**.

   > **Note**: **Time between re-evaluation of gates** represents the time interval between each evaluation of all the gates. At each sampling interval, new requests are sent concurrently to each gate for fresh results. The sampling interval must be greater than the longest typical response time of any configured gate to allow time for all responses to be received.

   - Set the value of **Timeout after which gates fail** to **8 Minutes**.

   > **Note**: **Timeout after which gates fail** represents the maximum evaluation period for all gates. The deployment will be rejected if the timeout is reached before all gates succeed during the same sampling interval. The minimum value we can specify for timeout is 6 minutes and 5 minutes for the sampling interval.

   > **Note**: In this case, when a release is triggered, the gate will validate the samples at *0th* and *5th* minutes. If the result is **Pass**, notification will be sent for approval. If the result is **Fail**, the release will time-out after *8th* minute.

   > **Note**: In reality these values can span multiple hours.

8. On the **Pre-deployment conditions** pane, select **On successful gates, ask for approvals** radio button.

   ![LAB10a-Az400_99](Evidencia/LAB10a-Az400_99.png)

9. Close the **Pre-deployment conditions** pane, by clicking the **x** mark in its upper right corner. **Save** the changes in the release pipeline.

   ![LAB10a-Az400_100](Evidencia/LAB10a-Az400_100.png)

   ![LAB10a-Az400_102](Evidencia/LAB10a-Az400_102.png)

10. For the **Query Work Items** gate to work, the **Project Build Service** requires Read permission for the Azure Board queries.

11. In the Azure DevOps portal, in the vertical navigational pane, hover the mouse pointer over **Boards** hold down the **Ctrl** key and click **Queries** to open a separate browser tab with the **Queries** pane.

    ![LAB10a-Az400_103](Evidencia/LAB10a-Az400_103.png)

12. On the **Queries** pane of the **Boards** view, click **All** to get a list of all queries.

    ![LAB10a-Az400_104](Evidencia/LAB10a-Az400_104.png)

13. Right-click the folder **Shared Queries** and select **Security…** to open the pane **Permissions for Shared Queries**.

    ![LAB10a-Az400_105](Evidencia/LAB10a-Az400_105.png)

14. On the **Permissions for Shared Queries** pane, into the field **Search for users or groups**, type or paste **Controlling Deployments using Release Gates Build Service** ([Project Name] Build Service) and click the one found identity.

    > **Note**: The user **Controlling Deployments using Release Gates Build Service** has to be searched for like described above, it does not already appear as a member of the **Users** list. Don’t confuse the user **Project Collection Build Service** with **Project Build Service**.

15. Select the user **Controlling Deployments using Release Gates Build Service** in the **Users** list and on the right hand site set the **Read** permission to **Allow**.

    ![LAB10a-Az400_106](Evidencia/LAB10a-Az400_106.png)

16. Close the **Permissions for Shared Queries** pane, by clicking the **x** mark in its upper right corner.

    ![LAB10a-Az400_107](Evidencia/LAB10a-Az400_107.png)

17. Navigate back to the browser tab where the release pipeline is still open.

#### Task 2: Configure post-deployment gates

In this task, you will enable the post-deployment gate for the Canary Environment.

1. Back on the **All pipelines > PartsUnlimited-CD** pane, on the right edge of the rectangle representing the **Canary Environment** stage, click the oval shape representing the **Post-deployment conditions**.

   ![LAB10a-Az400_108](Evidencia/LAB10a-Az400_108.png)

2. On **Post-deployment conditions** pane, set the **Gates** slider to **Enabled**, click **+ Add**, and, in the pop-up menu, click **Query Azure Monitor Alerts**.

3. On **Post-deployment conditions** pane, in the **Query Azure Monitor Alerts** section, in the **Azure subscription** dropdown list, select the entry representing your Azure subscription (under Available Azure service), and, in the **Resource group** dropdown list, select the **az400m10l01-RG** entry.

   ![LAB10a-Az400_109](Evidencia/LAB10a-Az400_109.png)

4. On **Post-deployment conditions** pane, expand the **Evaluation options** and configure the following options:

- Set the value of **Time between re-evaluation of gates** to **5 Minutes**.

- Set the value of **Timeout after which gates fail** to **8 Minutes**.

- Select the **On successful gates, ask for approvals** option.

  ![LAB10a-Az400_110](Evidencia/LAB10a-Az400_110.png)
  
  > **Note**: The sampling interval and timeout work together so that the gates will call their functions at suitable intervals and reject the deployment if they don’t succeed during the same sampling interval within the timeout period.

1. Close the **Post-deployment conditions** pane, by clicking the **x** mark in its upper right corner.

2. Back on the **PartsUnlimited-CD** pane, click **Save**, and in the **Save** dialog box, click **Save** again.

   ![LAB10a-Az400_111](Evidencia/LAB10a-Az400_111.png)

   ![LAB10a-Az400_112](Evidencia/LAB10a-Az400_112.png)

   ![LAB10a-Az400_113](Evidencia/LAB10a-Az400_113.png)

### Exercise 3: Test release gates

In this exercise, you will test the release gates by updating the application, which will trigger a deployment.

#### Task 1: Update and deploy application after adding release gates

In this task, you will make a minor change in the application code, commit the update to the repository, and track the build and release process.

1. In the browser window displaying the Azure DevOps portal, in the vertical navigational pane, select **Releases**.

2. Click on **Create release** and **Create** (leave defaults).

   ![LAB10a-Az400_114](Evidencia/LAB10a-Az400_114.png)

   ![LAB10a-Az400_115](Evidencia/LAB10a-Az400_115.png)

   ![LAB10a-Az400_116](Evidencia/LAB10a-Az400_116.png)

3. On the pane representing the most recent run, click the **Releases** tab and then click the **PartsUnlimited-CD/Release-2** entry and review the progress of the deployment to the **Canary Environment**.

   ![LAB10a-Az400_117](Evidencia/LAB10a-Az400_117.png)

   ![LAB10a-Az400_118](Evidencia/LAB10a-Az400_118.png)

4. Click the oval shape representing the **Pre-deployment conditions** on the left edge of the rectangle representing the **Canary Environment** stage, which, at this point, might be labeled either **Evaluating gates** or **Pre-deployment gates failed**.

   ![LAB10a-Az400_119](Evidencia/LAB10a-Az400_119.png)

5. On the **Canary Environment** pane, note that the **Query Work Items** gate failed.

   ![LAB10a-Az400_120](Evidencia/LAB10a-Az400_121.png)

   > **Note**: This indicates that there are active work items. These work items need to be closed in order to proceed further. Next sampling time will be after 5 minutes.

6. Open a new browser tab, navigate to the Azure DevOps portal, in the vertical navigational pane, select **Boards** and, in the **Boards** section, select **Queries**.

7. On the **Queries** pane of the **Boards** view, click the **All** tab.

   ![LAB10a-Az400_122](Evidencia/LAB10a-Az400_122.png)

8. On the **All** tab of the **Queries** pane, in the **Shared Queries** section, click the **Bugs** entry, on the **Queries > Shared Queries > Bugs** pane, and click **Run query**.

   ![LAB10a-Az400_123](Evidencia/LAB10a-Az400_123.png)

9. Verify that the query returns a work item titled **Disk out of space in Canary Environment** in the **New** state.

   > **Note**: Let’s assuming that the infrastructure team has fixed the disk space issue.

10. Click the **Disk out of space in Canary Environment** entry.

    ![LAB10a-Az400_124](Evidencia/LAB10a-Az400_124.png)

11. On the **Disk out of space in Canary Environment** pane, in the upper left corner, next to the **State** label, click **New**, in the dropdown list, click **Closed** and then click **Save**.

    ![LAB10a-Az400_125](Evidencia/LAB10a-Az400_125.png)

    ![LAB10a-Az400_126](Evidencia/LAB10a-Az400_126.png)

    ![LAB10a-Az400_128](Evidencia/LAB10a-Az400_128.png)

12. Switch back to the **Canary Environment** pane and wait for the second evaluation to pass.

    ![LAB10a-Az400_129](Evidencia/LAB10a-Az400_129.png)

    ![LAB10a-Az400_130](Evidencia/LAB10a-Az400_130.png)

    > **Note**: In case the second evaluation already failed, hover with the mouse pointer over the rectangle representing the **Canary Environment** stage to reveal the **Redeploy** option, click **Redeploy**, and, on the **Canary Environment** blade, click **Deploy** and monitor the status of processing the pre-deployment gates.

    > **Note**: Once the evaluation is successful, you will see the request for pre-deployment approval.

    ![LAB10a-Az400_131](Evidencia/LAB10a-Az400_131.png)

13. Click on **Approve** to deploy in Canary environment

    ![LAB10a-Az400_132](Evidencia/LAB10a-Az400_132.png)

    ![LAB10a-Az400_133](Evidencia/LAB10a-Az400_133.png)

    ![LAB10a-Az400_134](Evidencia/LAB10a-Az400_134.png)

    > **Note**: Once the deployment to Canary environment is successful, we will see the post-deployment gates in action which uses Application Insights to detect presence of failed requests targeting the newly deployed application.

    ![LAB10a-Az400_135](Evidencia/LAB10a-Az400_135.png)

    ![LAB10a-Az400_136](Evidencia/LAB10a-Az400_136.png)

    ![LAB10a-Az400_137](Evidencia/LAB10a-Az400_137.png)

    ![LAB10a-Az400_138](Evidencia/LAB10a-Az400_138.png)

    ![LAB10a-Az400_139](Evidencia/LAB10a-Az400_139.png)

14. To trigger a failed request, switch to the web browser window displaying the Azure portal, navigate to the **Canary** Azure web app blade, and click **Browse**. This will open a new browser tab displaying the PartsUnlimited web site.

    ![LAB10a-Az400_141](Evidencia/LAB10a-Az400_141.png)

15. On the PartsUnlimited web site, click **More**.

    ![LAB10a-Az400_142](Evidencia/LAB10a-Az400_142.png)

    > **Note**: This part of web site is intentionally misconfigured, so it will trigger a failed request.

    ![LAB10a-Az400_143](Evidencia/LAB10a-Az400_143.png)

16. Return to the home page of the PartsUnlimited web site, click **More** again, and repeat this step a few more times.

    ![LAB10a-Az400_144](Evidencia/LAB10a-Az400_144.png)

    ![LAB10a-Az400_145](Evidencia/LAB10a-Az400_145.png)

    ![LAB10a-Az400_146](Evidencia/LAB10a-Az400_146.png)

    ![LAB10a-Az400_147](Evidencia/LAB10a-Az400_147.png)

    ![LAB10a-Az400_148](Evidencia/LAB10a-Az400_148.png)

    ![LAB10a-Az400_149](Evidencia/LAB10a-Az400_149.png)

    ![LAB10a-Az400_150](Evidencia/LAB10a-Az400_150.png)

    

17. Validate that failed requests were detected by Application Insights by navigating to the Application Insights blade of the **Canary** web app page, and, on the Application Insights blade, click **Alerts**, and verify that the page lists one or more **Sev 3** alerts.

    ![LAB10a-Az400_151](Evidencia/LAB10a-Az400_151.png)

    ![LAB10a-Az400_152](Evidencia/LAB10a-Az400_152.png)

    > **Note**: Since there is an alert triggered by the exception, **Query Azure Monitor** gate will fail. This, in turn, will prevent deployment to the **Production** environment.

### Exercise 4: Remove the Azure lab resources

In this exercise, you will remove the Azure resources provisioned in this lab to eliminate unexpected charges.

> **Note**: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges.

#### Task 1: Remove the Azure lab resources

In this task, you will use Azure Cloud Shell to remove the Azure resources provisione in this lab to eliminate unnecessary charges.

1. In the Azure portal, open the **Bash** shell session within the **Cloud Shell** pane.

2. List all resource groups created throughout the labs of this module by running the following command:

   ShellCopy

   ```sh
   az group list --query "[?starts_with(name,'az400m10l01-RG')].name" --output tsv
   ```

   ![LAB10a-Az400_153](Evidencia/LAB10a-Az400_153.png)

3. Delete all resource groups you created throughout the labs of this module by running the following command:

   ShellCopy

   ```sh
   az group list --query "[?starts_with(name,'az400m10l01-RG')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

   > **Note**: The command executes asynchronously (as determined by the –nowait parameter), so while you will be able to run another Azure CLI command immediately afterwards within the same Bash session, it will take a few minutes before the resource groups are actually removed.

![LAB10a-Az400_154](Evidencia/LAB10a-Az400_154.png)

![LAB10a-Az400_155](Evidencia/LAB10a-Az400_155.png)

![LAB10a-Az400_157](Evidencia/LAB10a-Az400_157.png)

## Review

In this lab, you configured release pipelines and then configured and tested release gates.