# Lab 08: Implementing GitHub Actions by using DevOps Starter

# Student lab manual

## Lab overview

In this lab, you will learn how to implement a GitHub Action workflow that deploys an Azure web app by using DevOps Starter.

## Objectives

After you complete this lab, you will be able to:

- Implement a GitHub Action workflow by using DevOps Starter
- Explain the basic characteristics of GitHub Action workflows

## Lab duration

- Estimated time: **30 minutes**

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

#### Prepare a GitHub account

If you don’t already have a GitHub account that you can use for this lab, follow instructions available at [Signing up for a new GitHub account](https://github.com/join) to create one.

### Exercise 1 : Create a DevOps Starter project

In this exercise, you will use DevOps Starter to facilitate provisioning of a number of resources, including:

- A GitHub repository hosting:
  - Code of a sample .NET Core web site.
  - Azure Resource Manager templates that deploy an Azure web app hosting the web site code.
  - A workflow that builds, deploys, and tests the web site.
- An Azure web app, automatically deployed by using the GitHub workflow.

#### Task 1: Create DevOps Starter project

In this task, you will create an Azure DevOps Starter project that automatically sets up a GitHub repository, as well as creates and triggers a GitHub workflow that deploys an Azure web app, based on the content of the GitHub repository.

1. From the lab computer, start a web browser, navigate to the [**Azure Portal**](https://portal.azure.com/), and sign in with the user account that has at least the Contributor role in the Azure subscription you are using in this lab.

   ![LAB08-Az400_01](Evidencia/LAB08-Az400_01.png)

   ![LAB08-Az400_02](Evidencia/LAB08-Az400_02.png)

   ![LAB08-Az400_03](Evidencia/LAB08-Az400_03.png)

   ![LAB08-Az400_04](Evidencia/LAB08-Az400_04.png)

   ![LAB08-Az400_05](Evidencia/LAB08-Az400_05.png)

   ![LAB08-Az400_06](Evidencia/LAB08-Az400_06.png)

   ![LAB08-Az400_07](Evidencia/LAB08-Az400_07.png)

   ![LAB08-Az400_08](Evidencia/LAB08-Az400_08.png)

   ![LAB08-Az400_09](Evidencia/LAB08-Az400_09.png)

   ![LAB08-Az400_10](Evidencia/LAB08-Az400_10.png)

   ![LAB08-Az400_11](Evidencia/LAB08-Az400_11.png)

   ![LAB08-Az400_12](Evidencia/LAB08-Az400_12.png)

   ![LAB08-Az400_13](Evidencia/LAB08-Az400_13.png)

   ![LAB08-Az400_14](Evidencia/LAB08-Az400_14.png)

   ![LAB08-Az400_15](Evidencia/LAB08-Az400_15.png)

2. In the Azure portal, search for and select the **DevOps Starter** resource type and, on the **DevOps Starter** blade, click either **+ Add**, **+ New**, or **+ Create**.

   ![LAB08-Az400_16](Evidencia/LAB08-Az400_16.png)

   ![LAB08-Az400_17](Evidencia/LAB08-Az400_17.png)

3. On the **Start fresh with a new application** page of the **DevOps Starter** blade, click the **here** link in the **Setting up DevOps starter with GitHub, click here** text.

   ![LAB08-Az400_18](Evidencia/LAB08-Az400_18.png)

   > **Note**: This will display the **DevOps starter settings** blade.

4. On the **DevOps starter settings** blade, ensure that the **GitHub** tile is selected and click **Done**.

   ![LAB08-Az400_19](Evidencia/LAB08-Az400_19.png)

5. Back on the **DevOps Starter** blade, click **Next: Framework >**.

   ![LAB08-Az400_20](Evidencia/LAB08-Az400_20.png)

6. On the **Choose an application framework** page of the **DevOps Starter** blade, select the **ASP.NET Core** tile and click **Next: Service >**.

   ![LAB08-Az400_22](Evidencia/LAB08-Az400_22.png)

7. On the **Select an Azure service to deploy the application** page of the **DevOps Starter** blade, ensure that the **Windows Web App** tile is selected and click **Next: Create >**.

   ![LAB08-Az400_23](Evidencia/LAB08-Az400_23.png)

8. On the **Select Repository and Subscription** page of the **DevOps Starter** blade, click **Authorize**.

   ![LAB08-Az400_24](Evidencia/LAB08-Az400_24.png)

   > **Note**: This will display the **Authorize Azure GitHub Actions** pop-up web browser window.

9. In the **Authorize Azure GitHub Actions** pop-up window, review the required permissions and click **Authorize AzureGithubActions**.

   ![LAB08-Az400_25](Evidencia/LAB08-Az400_25.png)

   ![LAB08-Az400_26](Evidencia/LAB08-Az400_26.png)

   ![LAB08-Az400_27](Evidencia/LAB08-Az400_27.png)

   ![LAB08-Az400_28](Evidencia/LAB08-Az400_28.png)

   > **Note**: This will redirect the web pop-up browser window to the Azure DevOps site, prompting you for your Azure DevOps information.

10. When prompted, in the pop-up web browser window, click **Continue**.

11. Back on the **Select Repository and Subscription** page of the **DevOps Starter** blade, specify the following settings and click **Review + Create**:

    | Setting      | Value                                                        |
    | :----------- | :----------------------------------------------------------- |
    | Organization | the name of GitHub account                                   |
    | Repository   | **az400m08l01**                                              |
    | Subscription | the name of the Azure subscription you are using for this lab |
    | Web app name | any valid, globally unique host name in the **azurewebsites.net** DNS namespace |
    | Location     | the name of any Azure region in which you can provision an Azure web app |

    > **Note**: Wait for the provisioning to complete. This should take about 1 minute.

    ![LAB08-Az400_29](Evidencia/LAB08-Az400_29.png)

    

12. On the **Deploy_DevOps_Project_az400m08l01 | Overview** blade, click **Go to Resource**.

    ![LAB08-Az400_30](Evidencia/LAB08-Az400_30.png)

    

13. On the **az400m08l01** blade, on the **GitHub Workflow** tile, click **Authorize**.

    ![LAB08-Az400_31](Evidencia/LAB08-Az400_31.png)

14. On the **GitHub Authorization** blade, click **Authorize** again.

    ![LAB08-Az400_32](Evidencia/LAB08-Az400_32.png)

15. Back on the **az400m08l01** blade, monitor the progress of actions on the **GitHub Workflow** tile.

    ![LAB08-Az400_33](Evidencia/LAB08-Az400_33.png)

    ![LAB08-Az400_34](Evidencia/LAB08-Az400_34.png)

    ![LAB08-Az400_35](Evidencia/LAB08-Az400_35.png)

    ![LAB08-Az400_36](Evidencia/LAB08-Az400_36.png)

    ![LAB08-Az400_37](Evidencia/LAB08-Az400_37.png)

    ![LAB08-Az400_38](Evidencia/LAB08-Az400_38.png)

    > **Note**: Wait for the build, deployment, and functional tests jobs of the GitHub workflow to complete. This should take about 5 minutes.

#### Task 2: Review the results of creating the DevOps Starter project

In this task, you will review the results of creating the DevOps Starter project.

1. In the web browser window displaying the Azure portal, on the **az400m08l01** blade, review the **GitHub Workflow** section and verify that the **build**, **deploy**, and **Functional tests** jobs completed successfully.

   ![LAB08-Az400_39](Evidencia/LAB08-Az400_39.png)

   ![LAB08-Az400_40](Evidencia/LAB08-Az400_40.png)

   ![LAB08-Az400_41](Evidencia/LAB08-Az400_41.png)

   ![LAB08-Az400_42](Evidencia/LAB08-Az400_42.png)

   ![LAB08-Az400_43](Evidencia/LAB08-Az400_43.png)

   ![LAB08-Az400_44](Evidencia/LAB08-Az400_44.png)

   ![LAB08-Az400_45](Evidencia/LAB08-Az400_45.png)

   ![LAB08-Az400_46](Evidencia/LAB08-Az400_46.png)

   ![LAB08-Az400_47](Evidencia/LAB08-Az400_47.png)

   ![LAB08-Az400_48](Evidencia/LAB08-Az400_48.png)

   ![LAB08-Az400_49](Evidencia/LAB08-Az400_49.png)

   ![LAB08-Az400_50](Evidencia/LAB08-Az400_50.png)

   ![LAB08-Az400_51](Evidencia/LAB08-Az400_51.png)

2. On the **az400m08l01** blade, review the **Azure resources** section and verify that they include an App Service web app instance and the corresponding Application Insights resource.

3. At the top of the **az400m08l01** blade, note the link to the **Workflow file** and the GitHub repository you created in the previous task.

   ![LAB08-Az400_52](Evidencia/LAB08-Az400_52.png)

4. At the top of the **az400m08l01** blade, click the link to the GitHub repository.

   ![LAB08-Az400_53](Evidencia/LAB08-Az400_53.png)

5. On the GitHub repository page, note the three folders labeled:
   - **.github\workflows** - containing the YAML-formatted workflow file

   - **Application** - containing the code of the sample web site

   - **ArmTemplates** - containing the Azure Resource Manager templates that the workflow uses to provision Azure resources

     ![LAB08-Az400_54](Evidencia/LAB08-Az400_54.png)

6. On the GitHub repository page, click **.github/workflows** and then, click the **devops-starter-workflow.yml** entry.

   ![LAB08-Az400_55](Evidencia/LAB08-Az400_55.png)

7. On the GitHub repository page displaying the content of the **devops-starter-workflow.yml**, review its content and note that it contains the **build**, **deploy**, and **Functional tests** job definitions.

   ![LAB08-Az400_56](Evidencia/LAB08-Az400_56.png)

   ![LAB08-Az400_57](Evidencia/LAB08-Az400_57.png)

   ![LAB08-Az400_58](Evidencia/LAB08-Az400_58.png)

8. On the GitHub repository page, in the toolbar, click **Actions**.

   ![LAB08-Az400_59](Evidencia/LAB08-Az400_59.png)

9. On the GitHub repository page, on the **Actions** tab, in the **All workflows** section, click the entry representing the most recent workflow run.

   ![LAB08-Az400_60](Evidencia/LAB08-Az400_60.png)

10. On the workflow run page, review the workflow status, as well as the listing of **Annotations** and **Artifacts**.

    ![LAB08-Az400_61](Evidencia/LAB08-Az400_61.png)

    ![LAB08-Az400_62](Evidencia/LAB08-Az400_62.png)

11. On the GitHub repository page, in the toolbar, click **Settings** and, on the **Settings** tab, click **Secrets**.

    ![LAB08-Az400_63](Evidencia/LAB08-Az400_63.png)

    ![LAB08-Az400_64](Evidencia/LAB08-Az400_64.png)

12. On the **Action secrets** pane, note the **AZURE_CREDENTIALS** entry, representing the credentials necessary to access the target Azure subscription.

    ![LAB08-Az400_65](Evidencia/LAB08-Az400_65.png)

13. Navigate to the **az400m08l01/Application/aspnet-core-dotnet-core/Pages/Index.cshtml** GitHub repository page and, in the upper right corner, click the pencil icon to switch to the edit mode.

    ![LAB08-Az400_66](Evidencia/LAB08-Az400_66.png)

    ![LAB08-Az400_67](Evidencia/LAB08-Az400_67.png)

14. Change the line 19 to `<div class="description line-1"> GitHub Workflow has been successfully updated</div>`.

    ![LAB08-Az400_68](Evidencia/LAB08-Az400_68.png)

15. Scroll down to the bottom of the page and click **Commit changes**.

    ![LAB08-Az400_69](Evidencia/LAB08-Az400_69.png)

    ![LAB08-Az400_70](Evidencia/LAB08-Az400_70.png)

16. On the GitHub repository page, in the toolbar, click **Actions**.

17. In the **All workflows** section, click the **Update Index.cshtml** entry.

    ![LAB08-Az400_71](Evidencia/LAB08-Az400_71.png)

18. In the **devops-starter-workflow.yml** section, monitor the progress of deployment and verify that it completed successfully.

    ![LAB08-Az400_72](Evidencia/LAB08-Az400_72.png)

    ![LAB08-Az400_73](Evidencia/LAB08-Az400_73.png)

    ![LAB08-Az400_74](Evidencia/LAB08-Az400_74.png)

    ![LAB08-Az400_75](Evidencia/LAB08-Az400_75.png)

    ![LAB08-Az400_76](Evidencia/LAB08-Az400_76.png)

19. Switch to the browser window displaying the DevOps Starter blade in the Azure portal and click the **Browse** link next to the **Application endpoint** entry.

    ![LAB08-Az400_77](Evidencia/LAB08-Az400_77.png)

20. In the newly opened web browser window, verify that the updated text representing the change you committed in the GitHub repo is displayed on the web app home page.

    ![LAB08-Az400_78](Evidencia/LAB08-Az400_78.png)

### Exercise 2: Remove the Azure lab resources

In this exercise, you will remove the Azure resources provisioned in this lab to eliminate unexpected charges.

> **Note**: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges.

#### Task 1: Remove the Azure lab resources

In this task, you will use Azure Cloud Shell to remove the Azure resources provisioned in this lab to eliminate unnecessary charges.

1. In the Azure portal, open the **Bash** shell session within the **Cloud Shell** pane.

2. List all resource groups created throughout the labs of this module by running the following command:

   ShellCopy

   ```sh
   az group list --query "[?starts_with(name,'az400m08l01')].name" --output tsv
   ```

   ![LAB08-Az400_79](Evidencia/LAB08-Az400_79.png)

3. Delete all resource groups you created throughout the labs of this module by running the following command:

   ShellCopy

   ```sh
   az group list --query "[?starts_with(name,'az400m08l01')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

   > **Note**: The command executes asynchronously (as determined by the –nowait parameter), so while you will be able to run another Azure CLI command immediately afterwards within the same Bash session, it will take a few minutes before the resource groups are actually removed.

![LAB08-Az400_80](Evidencia/LAB08-Az400_80.png)

![LAB08-Az400_81](Evidencia/LAB08-Az400_81.png)

![LAB08-Az400_82](Evidencia/LAB08-Az400_82.png)

## Review

In this lab, you implemented a GitHub Action workflow that deploys an Azure web app by using DevOps Starter.