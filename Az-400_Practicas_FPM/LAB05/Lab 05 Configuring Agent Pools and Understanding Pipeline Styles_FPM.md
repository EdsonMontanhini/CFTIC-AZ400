# Lab 05: Configuring Agent Pools and Understanding Pipeline Styles

# Student lab manual

## Lab overview

YAML-based pipelines allow you to fully implement CI/CD as code, in which pipeline definitions reside in the same repository as the code that is part of your Azure DevOps project. YAML-based pipelines support a wide range of features that are part of the classic pipelines, such as pull requests, code reviews, history, branching, and templates.

Regardless of the choice of the pipeline style, to build your code or deploy your solution by using Azure Pipelines, you need an agent. An agent hosts compute resources that runs one job at a time. Jobs can be run directly on the host machine of the agent or in a container. You have an option to run your jobs using Microsoft-hosted agents, which are managed for you, or implementing a self-hosted agent that you set up and manage on your own.

In this lab, you will step through the process of converting a classic pipeline into a YAML-based one and running it first by using a Microsoft-hosted agent and then performing the equivalent task by using a self-hosted agent.

## Objectives

After you complete this lab, you will be able to:

- implement YAML-based pipelines
- implement self-hosted agents

## Lab duration

- Estimated time: **45 minutes**

## Instructions

### Before you start

#### Sign in to the lab virtual machine

Ensure that you’re signed in to your Windows 10 computer by using the following credentials:

- Username: **Student**
- Password: **Pa55w.rd**

#### Review the installed applications

Find the taskbar on your Windows 10 desktop. The taskbar contains the icons for the applications that you’ll use in this lab:

- Microsoft Edge
- [Visual Studio Code](https://code.visualstudio.com/). This will be installed as part of prerequisites for this lab.

#### Set up an Azure DevOps organization

If you don’t already have an Azure DevOps organization that you can use for this lab, create one by following the instructions available at [Create an organization or project collection](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/create-organization?view=azure-devops).

### Exercise 0: Configure the lab prerequisites

In this exercise, you will set up the prerequisite for the lab, which consists of the preconfigured Parts Unlimited team project based on an Azure DevOps Demo Generator template.

#### Task 1: Configure the team project

In this task, you will use Azure DevOps Demo Generator to generate a new project based on the **PartsUnlimited** template.

1. On your lab computer, start a web browser and navigate to [Azure DevOps Demo Generator](https://azuredevopsdemogenerator.azurewebsites.net/). This utility site will automate the process of creating a new Azure DevOps project within your account that is prepopulated with content (work items, repos, etc.) required for the lab.

   > **Note**: For more information on the site, see https://docs.microsoft.com/en-us/azure/devops/demo-gen.

2. Click **Sign in** and sign in using the Microsoft account associated with your Azure DevOps subscription.

   ![LAB05-Az400_01](Evidencia/LAB05-Az400_01.png)

3. If required, on the **Azure DevOps Demo Generator** page, click **Accept** to accept the permission requests for accessing your Azure DevOps subscription.

4. On the **Create New Project** page, in the **New Project Name** textbox, type **Configuring Agent Pools and Understanding Pipeline Styles**, in the **Select organization** dropdown list, select your Azure DevOps organization, and then click **Choose template**.

   ![LAB05-Az400_02](Evidencia/LAB05-Az400_02.png)

5. On the **Choose a template** page, click the **PartsUnlimited** template, and then click **Select Template**.

   ![LAB05-Az400_03](Evidencia/LAB05-Az400_03.png)

6. Click **Create Project**

   ![LAB05-Az400_04](Evidencia/LAB05-Az400_04.png)

   ![LAB05-Az400_05](Evidencia/LAB05-Az400_05.png)

   ![LAB05-Az400_06](Evidencia/LAB05-Az400_06.png)

   ![LAB05-Az400_07](Evidencia/LAB05-Az400_07.png)

   ![LAB05-Az400_08](Evidencia/LAB05-Az400_08.png)

   > **Note**: Wait for the process to complete. This should take about 2 minutes. In case the process fails, navigate to your DevOps organization, delete the project, and try again.

7. On t![LAB05-Az400_09](Evidencia/LAB05-Az400_09.png)he **Create New Project** page, click **Navigate to project**.

   

### Exercise 1: Author YAML-based Azure DevOps pipelines

In this exercise, you will convert a classic Azure DevOps pipeline into a YAML-based one.

#### Task 1: Create an Azure DevOps YAML pipeline

In this task, you will create a template-based Azure DevOps YAML pipeline.

1. From the web browser displaying the Azure DevOps portal with the **Configuring Agent Pools and Understanding Pipeline Styles** project open, in the vertical navigational pane on the left side, click **Pipelines**.

2. On the **Recent** tab of the **Pipelines** pane, click **New pipeline**.

   ![LAB05-Az400_10](Evidencia/LAB05-Az400_10.png)

3. On the **Where is your code?** pane, click **Azure Repos Git**.

   ![LAB05-Az400_11](Evidencia/LAB05-Az400_11.png)

4. On the **Select a repository** pane, click **PartsUnlimited**.

   ![LAB05-Az400_12](Evidencia/LAB05-Az400_12.png)

5. On the **Configure your pipeline** pane, click **Starter pipeline**.

   ![LAB05-Az400_13](Evidencia/LAB05-Az400_13.png)

6. On the **Review your pipeline YAML** pane, review the sample pipeline, click the down-facing caret symbol next to the **Save and run** button, click **Save** and, on the **Save** pane, click **Save**.

   ![LAB05-Az400_14](Evidencia/LAB05-Az400_14.png)

   ![LAB05-Az400_15](Evidencia/LAB05-Az400_15.png)

   ![LAB05-Az400_16](Evidencia/LAB05-Az400_16.png)

   > **Note**: This will result in creation of the **azure-pipelines.yml** file in the root directory of the repository hosting the project code.

   > **Note**: You will replace the content of the sample YAML pipeline with the code of pipeline generated by the classic editor and modify it to account for differences between the classic and YAML pipelines.

#### Task 2: Convert a classic pipeline into a YAML pipeline

In this task, you will convert a classic pipeline into a YAML pipeline

1. From the web browser displaying the Azure DevOps portal with the **Configuring Agent Pools and Understanding Pipeline Styles** project open, in the vertical navigational pane on the left side, click **Pipelines**.

   ![LAB05-Az400_17](Evidencia/LAB05-Az400_17.png)

2. On the **Recent** tab of the **Pipelines** pane, hover with the mouse pointer over the right edge of the entry containing the **PartsUnlimitedE2E** entry to reveal the vertical ellipsis symbol designating the **More** menu, click the ellipsis symbol, and, in the dropdown menu, click **Edit**. This will display the build pipeline that is part of the project you generated at the beginning of the lab.

   ![LAB05-Az400_18](Evidencia/LAB05-Az400_18.png)

   ![LAB05-Az400_19](Evidencia/LAB05-Az400_19.png)

3. On the **Tasks** tab of the **PartsUnlimitedE2E** edit pane, click **Triggers**, on the right side of the **PartsUnlimited** pane, uncheck the **Enable continuous integration** checkbox, click the down-facing caret next to the **Save & queue** button, in the dropdown menu, click **Save**, and in the **Save build pipeline** click **Save**.

   ![LAB05-Az400_20](Evidencia/LAB05-Az400_20.png)

   ![LAB05-Az400_21](Evidencia/LAB05-Az400_21.png)

   ![LAB05-Az400_22](Evidencia/LAB05-Az400_22.png)

   ![LAB05-Az400_23](Evidencia/LAB05-Az400_23.png)

   ![LAB05-Az400_24](Evidencia/LAB05-Az400_24.png)

   > **Note**: This will prevent from unintended execution of automatic build due to changes to the repository during this lab.

   > **Note**: Alternatively, you could simply delete the existing pipeline once you copy its content into the new one.

4. In the Azure DevOps portal, in the vertical navigational pane on the left side, in the **Pipelines** section, click **Pipelines**.

5. On the **Recent** tab of the **Pipelines** pane, click the **PartsUnlimitedE2E** entry.

   ![LAB05-Az400_25](Evidencia/LAB05-Az400_25.png)

6. On the **Runs** tab of the **PartsUnlimitedE2E** pane, in the upper right corner, click the vertical ellipsis (three vertical dots) symbol and, in the dropdown menu, click **Export to YAML**. This will automatically download the **PartsUnlimitedE2E.yml** file to your local **Downloads** folder.

   ![LAB05-Az400_26](Evidencia/LAB05-Az400_26.png)

   > **Note**: The **Export to YAML** feature replaces an older **View YAML** option available from the pipeline editor pane within the Azure DevOps portal, which was limited to viewing YAML content one job at a time. The new functionality leverages existing classic and YAML pipeline infrastructure, including YAML parsing library, which results in more accurate translation between the two. It supports the following pipeline components:

   - Single and multiple jobs
   - Checkout options
   - Execution plan parallelism
   - Timeout and name metadata
   - Demands
   - Schedules and other triggers
   - Pool selection, including jobs which differ from the default
   - All tasks and all inputs, including optimizing for default inputs
   - Job and step conditions
   - Task group unrolling

   > **Note**: The only components not covered by the new functionality are variables and timezone translation. Variables defined in YAML override variables set in the user interface of the Azure DevOps portal. If the **Export to YAML** feature detects presence of Classic pipeline variables, they will be explicitly included in the comments within the newly generated YAML pipeline definition. Similarly, since cron schedules in YAML are expressed in UTC, while classic schedules rely on the organization’s timezone, their presence is also included in the comments.

   > **Note**: For more information regarding this functionality, refer to [Replacing “View YAML”](https://devblogs.microsoft.com/devops/replacing-view-yaml/)

7. On the lab computer, start Visual Studio Code and use it to open the file **PartsUnlimitedE2E.yml**. The file should have the following content:

   CodeCopy

   ```yaml
   name: $(date:yyyyMMdd)$(rev:.r)
   jobs:
   - job: Phase_1
     displayName: Phase 1
     cancelTimeoutInMinutes: 1
     pool:
       vmImage: vs2017-win2016
     steps:
     - checkout: self
     - task: NuGetInstaller@0
       name: NuGetInstaller_1
       displayName: NuGet restore
       inputs:
         solution: '**\*.sln'
     - task: VSBuild@1
       name: VSBuild_2
       displayName: Build solution
       inputs:
         vsVersion: 15.0
         msbuildArgs: /p:DeployOnBuild=true /p:WebPublishMethod=Package /p:PackageAsSingleFile=true /p:SkipInvalidConfigurations=true /p:PackageLocation="$(build.stagingDirectory)" /p:IncludeServerNameInBuildInfo=True /p:GenerateBuildInfoConfigFile=true /p:BuildSymbolStorePath="$(SymbolPath)" /p:ReferencePath="C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\Extensions\Microsoft\Pex"
         platform: $(BuildPlatform)
         configuration: $(BuildConfiguration)
     - task: VSTest@1
       name: VSTest_3
       displayName: Test Assemblies
       inputs:
         testAssembly: '**\$(BuildConfiguration)\*test*.dll;-:**\obj\**'
         codeCoverageEnabled: true
         vsTestVersion: latest
         platform: $(BuildPlatform)
         configuration: $(BuildConfiguration)
     - task: CopyFiles@2
       name: CopyFiles1
       displayName: Copy Files
       inputs:
         SourceFolder: $(build.sourcesdirectory)
         Contents: '**/*.json'
         TargetFolder: $(build.artifactstagingdirectory)
     - task: PublishBuildArtifacts@1
       name: PublishBuildArtifacts_5
       displayName: Publish Artifact
       inputs:
         PathtoPublish: $(build.artifactstagingdirectory)
         TargetPath: '\\my\share\$(Build.DefinitionName)\$(Build.BuildNumber)'
   ...
   ```

   ![LAB05-Az400_27](Evidencia/LAB05-Az400_27.png)

8. In the Visual Studio Code window, in the top-level menu, click **Selection** and, in the dropdown menu, click **Select All**.

   ![LAB05-Az400_28](Evidencia/LAB05-Az400_28.png)

   ![LAB05-Az400_29](Evidencia/LAB05-Az400_29.png)

9. In the Visual Studio Code window, in the top-level menu, click **Edit** and, in the dropdown menu, click **Copy**.

   ![LAB05-Az400_30](Evidencia/LAB05-Az400_30.png)

   ![LAB05-Az400_31](Evidencia/LAB05-Az400_31.png)

   ![LAB05-Az400_32](Evidencia/LAB05-Az400_32.png)

   

10. Switch to the browser window displaying the Azure DevOps portal and, in the vertical navigational pane on the left side, in the **Pipelines** section, click **Pipelines**.

    ![LAB05-Az400_33](Evidencia/LAB05-Az400_33.png)

11. On the **Recent** tab of the **Pipelines** pane, select **PartsUnlimited** and, on the **PartsUnlimited** pane, select **Edit**.

    ![LAB05-Az400_34](Evidencia/LAB05-Az400_34.png)

    ![LAB05-Az400_35](Evidencia/LAB05-Az400_35.png)

    

12. On the **PartsUnlimited** edit pane, select the existing YAML content of the pipeline, replace it with the content of Clipboard.

    ![LAB05-Az400_36](Evidencia/LAB05-Az400_36.png)

    ![LAB05-Az400_34](Evidencia/LAB05-Az400_34.png)

    ![LAB05-Az400_35](Evidencia/LAB05-Az400_35.png)

    ![LAB05-Az400_37](Evidencia/LAB05-Az400_37.png)

    ![LAB05-Az400_38](Evidencia/LAB05-Az400_38.png)

    ![LAB05-Az400_39](Evidencia/LAB05-Az400_39.png)

13. On the **PartsUnlimited** edit pane, in the upper-right corner, click **Save**, and, on the **Save** pane, click **Save**. This will automatically trigger the build based on this pipeline.

    ![LAB05-Az400_40](Evidencia/LAB05-Az400_40.png)

    ![LAB05-Az400_41](Evidencia/LAB05-Az400_41.png)

    ![LAB05-Az400_42](Evidencia/LAB05-Az400_42.png)

14. In the Azure DevOps portal, in the vertical navigational pane on the left side, in the **Pipelines** section, click **Pipelines**.

15. On the **Recent** tab of the **Pipelines** pane, click the **PartsUnlimited** entry, on the **Runs** tab of the **PartsUnlimited** pane, select the most recent run, on the **Summary** pane of the run, scroll down to the bottom, in the **Jobs** section, click **Phase 1** and monitor the job until its successful completion.

    ![LAB05-Az400_43](Evidencia/LAB05-Az400_43.png)

    ![LAB05-Az400_44](Evidencia/LAB05-Az400_44.png)

    ![LAB05-Az400_45](Evidencia/LAB05-Az400_45.png)

    ![LAB05-Az400_46](Evidencia/LAB05-Az400_46.png)

    ![LAB05-Az400_47](Evidencia/LAB05-Az400_47.png)

### Exercise 2: Manage Azure DevOps agent pools

In this exercise, you will implement self-hosted Azure DevOps agent.

#### Task 1: Configure an Azure DevOps self-hosting agent

In this task, you will configure the LOD VM as an Azure DevOps self-hosting agent and use it to run a build pipeline.

1. Within the Lab Virtual machine (Lab VM) or your own computer, start a web browser, navigate to [the Azure DevOps portal](https://dev.azure.com/) and sign in by using the Microsoft account associated with your Azure DevOps organization.

2. In the Azure DevOps portal, in the upper right corner of the Azure DevOps page, click the **User settings** icon, in the dropdown menu, click **Personal access tokens**, on the **Personal Access Tokens** pane, and click **+ New Token**.

   ![LAB05-Az400_48](Evidencia/LAB05-Az400_48.png)

   ![LAB05-Az400_49](Evidencia/LAB05-Az400_49.png)

   ![LAB05-Az400_50](Evidencia/LAB05-Az400_50.png)

3. On the **Create a new personal access token** pane, click the **Show all scopes** link and, specify the following settings and click **Create** (leave all others with their default values):

   | Setting                | Value                                                        |
   | :--------------------- | :----------------------------------------------------------- |
   | Name                   | **Configuring Agent Pools and Understanding Pipeline Styles lab** |
   | Scope (custom defined) | **Agent Pools** (show more scopes option below if needed)    |
   | Permissions            | **Read and manage**                                          |

   ![LAB05-Az400_52](Evidencia/LAB05-Az400_52.png)

4. On the **Success** pane, copy the value of the personal access token to Clipboard.

   ![LAB05-Az400_53](Evidencia/LAB05-Az400_53.png)

   ![LAB05-Az400_54](Evidencia/LAB05-Az400_54.png)

   ![LAB05-Az400_55](Evidencia/LAB05-Az400_55.png)

   > **Note**: Make sure you copy the token. You will not be able to retrieve it once you close this pane.

5. On the **Success** pane, click **Close**.

6. On the **Personal Access Token** pane of the Azure DevOps portal, click **Azure DevOps** symbol in the upper left corner and then click **Organization settings** label in the lower left corner.

   ![LAB05-Az400_57](Evidencia/LAB05-Az400_57.png)

7. To the left side of the **Overview** pane, in the vertical menu, in the **Pipelines** section, click **Agent pools**.

   ![LAB05-Az400_58](Evidencia/LAB05-Az400_58.png)

8. On the **Agent pools** pane, in the upper right corner, click **Add pool**.

   ![LAB05-Az400_59](Evidencia/LAB05-Az400_59.png)

9. On the **Add agent pool** pane, in the **Pool type** dropdown list, select **Self-hosted**, in the **Name** text box, type **az400m05l05a-pool** and then click **Create**.

   ![LAB05-Az400_60](Evidencia/LAB05-Az400_60.png)

10. Back on the **Agent pools** pane, click the entry representing the newly created **az400m05l05a-pool**.

    ![LAB05-Az400_61](Evidencia/LAB05-Az400_61.png)

    

11. On the **Jobs** tab of the **az400m05l05a-pool** pane, click the **Agents** header.

12. On the **Agents** tab of the **az400m05l05a-pool** pane, click the **New agent** button.

    ![LAB05-Az400_62](Evidencia/LAB05-Az400_62.png)

    

13. On the **Get the agent** pane, ensure that the **Windows** and **x64** tabs are selected, and click **Download** to download the zip archive containing the agent binaries to download it into the local **Downloads** folder within your user profile.

    ![LAB05-Az400_63](Evidencia/LAB05-Az400_63.png)

    ![LAB05-Az400_64](Evidencia/LAB05-Az400_64.png)

    > **Note**: If you receive an error message at this point indicating that the current system settings prevent you from downloading the file, in the Internet Explorer window, in the upper right corner, click the gearwheel symbol designating the **Settings** menu header, in the dropdown menu, select **Internet Options**, in the **Internet Options** dialog box, click **Advanced**, on the **Advanced** tab, click **Reset**, in the **Reset Internet Explorer Settings** dialog box, click **Reset** again, click **Close**, and try the download again.

14. Start Windows PowerShell as administrator and in the **Administrator: Windows PowerShell** console run the following lines to create the **C:\agent** directory and extract the content of the downloaded archive into it.

    CodeCopy

    ```powershell
    cd \
    mkdir agent ; cd agent
    $TARGET = Get-ChildItem "$Home\Downloads\vsts-agent-win-x64-*.zip"
    Add-Type -AssemblyName System.IO.Compression.FileSystem
    [System.IO.Compression.ZipFile]::ExtractToDirectory($TARGET, "$PWD")
    ```

    ![LAB05-Az400_66](Evidencia/LAB05-Az400_66.png)

15. In the same **Administrator: Windows PowerShell** console, run the following to configure the agent:

    CodeCopy

    ```powershell
    .\config.cmd
    ```

    

16. When prompted, specify the values of the following settings:

    | Setting                                                      | Value                                                        |
    | :----------------------------------------------------------- | :----------------------------------------------------------- |
    | Enter server URL                                             | the URL of your Azure DevOps organization, in the format **https://dev.azure.com/`<organization_name>`**, where `<organization_name>` represents the name of your Azure DevOps organization |
    | Enter authentication type (press enter for PAT)              | **Enter**                                                    |
    | Enter personal access token                                  | The access token you recorded earlier in this task           |
    | Enter agent pool (press enter for default)                   | **az400m05l05a-pool**                                        |
    | Enter agent name                                             | **az400m05-vm0**                                             |
    | Enter work folder (press enter for _work)                    | **Enter**                                                    |
    | Enter Perform an unzip for tasks for each step. (press enter for N) | **Enter**                                                    |
    | Enter run agent as service? (Y/N) (press enter for N)        | **Y**                                                        |
    | Enter User account to use for the service (press enter for NT AUTHORITY\NETWORK SERVICE) | **Enter**                                                    |

    > **Note**: You can run self-hosted agent as either a service or an interactive process. You might want to start with the interactive mode, since this simplifies verifying agent functionality. For production use, you should consider either running the agent as a service or as an interactive process with auto-logon enabled, since both persist their running state and ensure that the agent starts automatically if the operating system is restarted.

    > **Note**: Verify that the agent is reporting the **Listening for Jobs** status.

    ![LAB05-Az400_67](Evidencia/LAB05-Az400_67.png)

    ![LAB05-Az400_68](Evidencia/LAB05-Az400_68.png)

17. Switch to the browser window displaying the Azure DevOps portal and close the **Get the agent** pane.

18. Back on the **Agents** tab of the **az400m05l05a-pool** pane, note that the newly configured agent is listed with the **Online** status.

    ![LAB05-Az400_69](Evidencia/LAB05-Az400_69.png)

    ![LAB05-Az400_70_](Evidencia/LAB05-Az400_70_.png)

19. In the web browser window displaying the Azure DevOps portal, in the upper left corner, click the **Azure DevOps** label.

20. In the browser window displaying the list of projects, click the tile representing your **Configuring Agent Pools and Understanding Pipeline Styles** project.

    ![LAB05-Az400_71_](Evidencia/LAB05-Az400_71_.png)

    ![LAB05-Az400_74](Evidencia/LAB05-Az400_74.png)

21. On the **Configuring Agent Pools and Understanding Pipeline Styles** pane, in the vertical navigational pane on the left side, in the **Pipelines** section, click **Pipelines**.

22. On the **Recent** tab of the **Pipelines** pane, select **PartsUnlimited** and, on the **PartsUnlimited** pane, select **Edit**.

    ![LAB05-Az400_75](Evidencia/LAB05-Az400_75.png)

    ![LAB05-Az400_76](Evidencia/LAB05-Az400_76.png)

23. On the **PartsUnlimited** edit pane, in the existing YAML-based pipeline, replace line **7** `vmImage: vs2017-win2016` designating the target agent pool the following content, designating the newly created self-hosted agent pool:

    ![LAB05-Az400_77](Evidencia/LAB05-Az400_77.png)

    CodeCopy

    ```yaml
    name: az400m05l05a-pool
    demands:
    - agent.name -equals az400m05-vm0
    ```

    ![LAB05-Az400_78](Evidencia/LAB05-Az400_78.png)

    ![LAB05-Az400_79](Evidencia/LAB05-Az400_79.png)

24. For `Task: NugetInstaller@0`, click on **Settings (link that is displaying above the task in grey colour)**, modify **Advanced > NuGet Version > 4.0.0** and click on **Add**.

    ![LAB05-Az400_80](Evidencia/LAB05-Az400_80.png)

    ![LAB05-Az400_81](Evidencia/LAB05-Az400_81.png)

    ![LAB05-Az400_82](Evidencia/LAB05-Az400_82.png)

    

25. On the **PartsUnlimited** edit pane, in the upper right corner of the pane, click **Save** and, on the **Save** pane, click **Save** again. This will automatically trigger the build based on this pipeline.

    ![LAB05-Az400_83](Evidencia/LAB05-Az400_83.png)

    ![LAB05-Az400_85](Evidencia/LAB05-Az400_85.png)

26. In the Azure DevOps portal, in the vertical navigational pane on the left side, in the **Pipelines** section, click **Pipelines**.

27. On the **Recent** tab of the **Pipelines** pane, click the **PartsUnlimited** entry, on the **Runs** tab of the **PartsUnlimited** pane, select the most recent run, on the **Summary** pane of the run, scroll down to the bottom, in the **Jobs** section, click **Phase 1** and monitor the job until its successful completion.

    ![LAB05-Az400_86](Evidencia/LAB05-Az400_86.png)

![LAB05-Az400_87](Evidencia/LAB05-Az400_87.png)



Downloading the Release v.2.182.1 microsoft/azure-pipelines-agent

[Release v2.182.1 · microsoft/azure-pipelines-agent · GitHub](https://github.com/microsoft/azure-pipelines-agent/releases/tag/v2.182.1)

Features Skip task's post-execution step if target container stopped (#3230) Should ignore PSModulePath (#3231) Use %AZP25 instead of %25 for escaping (#3241) Added -y to yum updateinfo comman...

![LAB05-Az400_87a](Evidencia/LAB05-Az400_87a.png)

Adding a new line in **azure-pipelines.yml** with the new agent version

![LAB05-Az400_88](Evidencia/LAB05-Az400_88.png)

![LAB05-Az400_89](Evidencia/LAB05-Az400_89.png)

![LAB05-Az400_90](Evidencia/LAB05-Az400_90.png)

![LAB05-Az400_91](Evidencia/LAB05-Az400_91.png)

**Uninstalling and reinstalling the agent** several times and checking the pipeline performing

![LAB05-Az400_93](Evidencia/LAB05-Az400_93.png)

![LAB05-Az400_94](Evidencia/LAB05-Az400_94.png)

![LAB05-Az400_95](Evidencia/LAB05-Az400_95.png)

![LAB05-Az400_96](Evidencia/LAB05-Az400_96.png)

![LAB05-Az400_97](Evidencia/LAB05-Az400_97.png)

![LAB05-Az400_98](Evidencia/LAB05-Az400_98.png)

![LAB05-Az400_99](Evidencia/LAB05-Az400_99.png)

After a lot of test the error persists

![LAB05-Az400_100](Evidencia/LAB05-Az400_100.png)

**Installing Visual Studio 2019 Community**...

![LAB05-Az400_101](Evidencia/LAB05-Az400_101.png)

![LAB05-Az400_101a](Evidencia/LAB05-Az400_101a.png)



**Deletion of line 10 - agent.version –gtVersion 2.182.1 in azure-pipelines.yml**

![LAB05-Az400_103](Evidencia/LAB05-Az400_103.png)

![LAB05-Az400_104](Evidencia/LAB05-Az400_104.png)

![LAB05-Az400_105](Evidencia/LAB05-Az400_105.png)

![LAB05-Az400_106](Evidencia/LAB05-Az400_106.png)

![LAB05-Az400_107](Evidencia/LAB05-Az400_107.png)

![LAB05-Az400_108](Evidencia/LAB05-Az400_108.png)

![LAB05-Az400_109](Evidencia/LAB05-Az400_109.png)

![LAB05-Az400_110](Evidencia/LAB05-Az400_110.png)

![LAB05-Az400_111](Evidencia/LAB05-Az400_111.png)

![LAB05-Az400_112](Evidencia/LAB05-Az400_112.png)

![LAB05-Az400_113](Evidencia/LAB05-Az400_113.png)

![LAB05-Az400_114](Evidencia/LAB05-Az400_114.png)

![LAB05-Az400_115](Evidencia/LAB05-Az400_115.png)

![LAB05-Az400_116](Evidencia/LAB05-Az400_116.png)

![LAB05-Az400_117](Evidencia/LAB05-Az400_117.png)

![LAB05-Az400_118](Evidencia/LAB05-Az400_118.png)

![LAB05-Az400_119](Evidencia/LAB05-Az400_119.png)

![LAB05-Az400_120](Evidencia/LAB05-Az400_120.png)

![LAB05-Az400_121](Evidencia/LAB05-Az400_121.png)

![LAB05-Az400_122](Evidencia/LAB05-Az400_122.png)

![LAB05-Az400_123](Evidencia/LAB05-Az400_123.png)

#### Review

In this lab, you learned how to convert classic pipelines into YAML-based ones and how to implement and use self-hosted agents.