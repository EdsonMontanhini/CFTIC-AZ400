# Lab 06b: Integrating External Source Control with Azure Pipelines

# Student lab manual

## Lab overview

With the introduction of Azure DevOps, Microsoft is offering developers a new continuous integration/continuous delivery (CI/CD) service called Azure Pipelines that enables you to continuously build, test, and deploy to any platform or cloud. It has cloud-hosted agents for Linux, macOS, and Windows; powerful workflows with native container support; and flexible deployments to Kubernetes, VMs, and serverless environments.

Azure Pipelines provides unlimited CI/CD minutes and 10 parallel jobs to every GitHub open source project for free. All open source projects run on the same infrastructure that our paying customers use. That means you’ll have the same fast performance and high quality of service. Many of the top open source projects are already using Azure Pipelines for CI/CD, such as Atom, CPython, Pipenv, Tox, Visual Studio Code, and TypeScript-and the list is growing every day.

In this lab, you’ll see how easy it is to set up Azure Pipelines with your GitHub projects and how you can start seeing benefits immediately.

## Objectives

After you complete this lab, you will be able to:

- Install Azure Pipelines from the GitHub Marketplace.
- Integrate a GitHub project with an Azure DevOps pipeline.
- Track pull requests through the pipeline.

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

#### Set up an Azure DevOps organization

If you don’t already have an Azure DevOps organization that you can use for this lab, create one by following the instructions available at [Create an organization or project collection](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/create-organization?view=azure-devops).

#### Set up a GitHub account

If you don’t already have a GitHub account that you can use for this lab, follow instructions available at [Signing up for a new GitHub account](https://docs.github.com/en/free-pro-team@latest/github/getting-started-with-github/signing-up-for-a-new-github-account).

### Exercise 1: Getting started with Azure Pipelines

In this exercise, you will integrate a GitHub project with Azure DevOps by using the new Azure Pipelines integration from the Marketplace.

#### Task 1: Forking a GitHub repo and installing Azure Pipelines

In this task, you will fork a GitHub repo and install Azure Pipelines in your GitHub account.

1. On your lab computer, start a web browser, navigate to the [GitHub actionsdemos/calculator site](https://github.com/actionsdemos/calculator) and, if you’re not already signed in to GitHub, sign in now.

   ![LAB06b-Az400_01](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_01.png)

   ![LAB06b-Az400_02](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_02.png)

   > **Note**: This is the baseline project we will fork and use for this lab.

2. On the **actionsdemos/calculator site** page, click **Fork**, to fork the repository to your own GitHub account. If prompted, select the account to fork the repository into.

   ![LAB06b-Az400_03](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_03.png)

   ![LAB06b-Az400_04](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_04.png)

   ![LAB06b-Az400_05](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_05.png)

   

3. On the page displaying the forked repo, at the top menu, click Marketplace.

   ![LAB06b-Az400_06](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_06.png)

   ![LAB06b-Az400_07](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_07.png)

   > **Note**: The **GitHub Marketplace** provides a variety of tools from Microsoft and 3rd parties that help you extend your project workflows.

4. In the **Search for apps and actions**, type **Azure Pipelines**, press the **Enter** key, and, in the list of results, click **Azure Pipelines**.

   ![LAB06b-Az400_08](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_08.png)

5. On the **Azure Pipelines** page, click **Read more** and read through the benefits of Azure Pipelines.

   ![LAB06b-Az400_09](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_09.png)

   ![LAB06b-Az400_10](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_10.png)

   ![LAB06b-Az400_11](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_11.png)

   ![LAB06b-Az400_12](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_12.png)

   > **Note**: The Azure Pipelines offering is free for anyone to use for public repositories, and free for a single build queue if you’re using a private repository.

6. On the **Azure Pipelines** page, click **Install it for free**. If you have multiple **GitHub** accounts, select the one you forked the calculator to from the **Switch billing account** dropdown.

   ![LAB06b-Az400_13](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_13.png)

7. On the **Review your order** page, click **Complete order and begin installation**.

![LAB06b-Az400_14](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_14.png)

1. On the **Install Azure Pipelines** page, use the default option **All repositories** and click **Install**.

   ![LAB06b-Az400_15](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_15.png)

   > **Note**: You have the option to specify repositories to include, but for the purposes of this lab, just include all of them. Note that Azure DevOps requires the listed set of permissions to fulfill its services.

2. If prompted, authenticate with your GitHub password to continue.

   ![LAB06b-Az400_16](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_16.png)

   ![LAB06b-Az400_17](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_17.png)

3. When prompted, on the **Setup your Azure Pipelines project** page, in the **Select your Azure DevOps organization** dropdown list, select your Azure DevOps account and click **Create a new project**.

4. When prompted, on the **Setup your Azure Pipelines project** page, in the **Project name** textbox, type **Integrating External Source Control with Azure Pipelines**, leave the **Project visibility** set to **Private**, and click **Continue**.

   ![LAB06b-Az400_18](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_18.png)

5. On the **Azure Pipelines by Microsoft would like permission to** page, click **Authorize Azure Pipelines**.

   ![LAB06b-Az400_19](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_19.png)

### Task 2: Configuring your Azure Pipelines project

In this task, you will configure the Azure Pipelines project based on the fork of the GitHub repo you created in the previous task.

> **Note**: You are now on the Azure DevOps site and need to set up your Azure Pipelines project.

1. In the Azure DevOps portal, click on **Pipelines** in the navigation panel on the left.

2. Click on the **New pipeline** button at top right.

3. On the **Select a repository** pane of the **Pipelines** view in the Azure DevOps portal, select the fork of the GitHub calculator repository you created in the previous task.

   ![LAB06b-Az400_20](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_20.png)

   > **Note**: Azure Pipelines will analyze your project in an attempt to determine if any existing templates would be a good fit. In this case, the recommended template is for **Node.js**, which is perfect for our needs. Some alternative templates are also suggested, although the recommended one is the best for this lab.

4. On the **Configure your pipeline**, select **Node.js**.

   ![LAB06b-Az400_21](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_21.png)

   > **Note**: The build pipeline is defined as **YAML**, a markup syntax well-suited to defining processes like this because it allows you to manage the configuration of the pipeline like any other file in the repo. It’s a pretty simple template that identifies the pool to pull a VM from for building, the process to install Node.js for building, and the actual build itself.

5. On the **Review your pipeline YAML**, click **Save and run** to save the pipeline and queue a new build.

   ![LAB06b-Az400_22](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_22.png)

6. On the **Save and run** pane, accept the default settings and click **Save and run**.

   ![LAB06b-Az400_23](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_23.png)

   > **Note**: For the purposes of this lab, you can commit this new file directly to the master branch.

   > **Note**: It will take a moment for the pipeline to complete. During this time it will configure the build agent, pull in the source from GitHub, and build it according to the pipeline definition.

7. On the **Summary** tab of the build job’s pane, verify that the build completed successfully.

   ![LAB06b-Az400_24](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_24.png)

   ![LAB06b-Az400_25](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_25.png)

   ![LAB06b-Az400_26](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_26.png)

   ![LAB06b-Az400_27](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_27.png)

   ![LAB06b-Az400_28](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_28.png)

   ![LAB06b-Az400_29](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_29.png)

   ![LAB06b-Az400_30](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_30.png)

   

### Task 3: Modifying a YAML build pipeline definition

In this task, you will modify the YAML build definition in the forked GitHub repository and track the build process triggered by your modification.

> **Note**: While the default pipeline is a great start, it doesn’t do everything we would like to have automated. For example, it would be great if it also ran our tests to confirm that the changes don’t create bugs. Let’s return to GitHub where we can edit the YAML by hand.

1. On the **Summary** tab of the build job’s pane, next to the **Repository and version** label, right-click the entry representing the GitHub project repo hosting the fork you created earlier in this lab and select **Open link in new tab**. This will open a new browser tab, displaying the GitHub page with the content of the fork.

   ![LAB06b-Az400_31](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_31.png)

   ![LAB06b-Az400_32](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_32.png)

   > **Note**: Since this lab will involve stepping back and forth between GitHub and Azure DevOps, it’ll be easier to keep a browser tab open to each.

2. On the GitHub page displaying the content of the fork, locate and click the entry representing the file **azure-pipelines.yml**. This will automatically open the file and display its content.

   ![LAB06b-Az400_33](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_33.png)

   ![LAB06b-Az400_34](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_34.png)

3. On the **master/calculator/azure-pipelines.yml** page, in the upper right corner of the pane displaying the file content, click the **Edit this file** icon in the shape of a pencil.

   ![LAB06b-Az400_35](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_35.png)

   > **Note**: Our project already contains tests written using Mocha so we just need to execute them in our pipeline.

4. To add the test run, add the `npm test` command directly below the `npm run build` command, with the same indentation. In addition, update the `displayName` entry to `'npm install, build, and test'` to clearly indicate what each task of the build is doing:

   CodeCopy

   ```
     npm test
   displayName: 'npm install, build, and test'
   ```

   ![LAB06b-Az400_36](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_36.png)

   ![LAB06b-Az400_37](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_37.png)

5. Scroll to the bottom of the page, replace the default commit message with **Adding npm test**, and click **Commit changes**.

   ![LAB06b-Az400_38](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_38.png)

   ![LAB06b-Az400_39](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_39.png)

   ![LAB06b-Az400_40](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_40.png)

   > **Note**: Again, it’s acceptable to commit this change directly to the master branch considering that this is a lab environment.

6. Switch back to the browser tab displaying the **Azure DevOps** portal and use the breadcrumb navigation to navigate to the **Pipelines** pane of the **Pipelines** view.

   ![LAB06b-Az400_41](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_41.png)

7. Verify that the new build triggered by the update already appears on the **Recent** tab in the **Recently run pipelines** list. Click the entry corresponding to the pipeline, on the **Runs** tab, select the most recent run, and, in the **Jobs** section, click the **Job** entry.

   ![LAB06b-Az400_42](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_42.png)

   ![LAB06b-Az400_43](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_43.png)

   

8. On the pane displaying job details, click on individual tasks of the job and follow it through to completion.

   ![LAB06b-Az400_44](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_44.png)

   ![LAB06b-Az400_45](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_45.png)

   ![LAB06b-Az400_46](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_46.png)

   ![LAB06b-Az400_47](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_47.png)

### Task 4: Proposing a change via GitHub pull request

In this task, you will propose an invalid change and review the results of a build triggered by a pull request.

> **Note**: One of the great benefits of this pipeline setup is that we now have a quality gate that’s automatically run every time someone commits a change. This makes it much easier to manage a project that could have any number of contributions coming in at various levels of quality.

1. Switch back to the browser tab displaying the GitHub page displaying the content of the **azure-pipelines.yml** file, navigate back to the page listing content of the forked repo and click **Go to file**.

   ![LAB06b-Az400_48](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_48.png)

2. At the **calculator/** prompt, type **arithmeticController.js** and, in the list of results click **api/controllers/arithmeticController.js**. This will automatically redirect the browser session to the **master/calculator/api/controllers/arithmeticController.js** page, displaying the content of that file.

   ![LAB06b-Az400_49](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_49.png)

   ![LAB06b-Az400_50](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_50.png)

   ![LAB06b-Az400_51](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_51.png)

   > **Note**: This controller contains the core functionality of the app. However, the code for the **add** operation isn’t completely clear. Put yourself in the position of someone with good intentions, but a lack of experience with JavaScript. They might identify this as an opportunity to help out by cleaning up the code to make it better.

3. On the **master/calculator/api/controllers/arithmeticController.js** page, in the upper right corner of the pane displaying the file content, click the **Edit this file** icon in the shape of a pencil.

   ![LAB06b-Az400_52](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_52.png)

4. Change the line ` ‘add’: function(a,b) { return +a + +b },`to` ‘add’: function(a,b) { return a + b },`.

   ![LAB06b-Az400_53](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_53.png)

   ![LAB06b-Az400_54](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_54.png)

   > **Note**: This is an incorrect change that would result in invalid outcome.

5. Scroll to the bottom of the page, replace the default commit message with **Modifying the add function**, select **Create a new branch**, set its name to **addition-cleanup**, and click **Propose file change**.

   ![LAB06b-Az400_55](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_55.png)

   ![LAB06b-Az400_56](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_56.png)

6. On the **Open a pull request** page, click **Create pull request** to initiate the process of getting your untested changes into production code.

   ![LAB06b-Az400_57](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_57.png)

   ![LAB06b-Az400_58](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_58.png)

   ![LAB06b-Az400_59](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_59.png)

   ![LAB06b-Az400_60](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_60.png)

   > **Note**: Azure DevOps will detect the change and start the build pipeline. This will result in failed checks, triggering an update the GitHub UI.

   > **Note**: Return to your original mindset of “project owner”.

7. On the **Modifying the add function #1** pull request page, in the **All checks have failed** section, click **Details** to learn more.

   ![LAB06b-Az400_61](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_61.png)

8. Review the **ANNOTATIONS** section and click the link **View more details on Azure Pipelines** directly below it. This will open a new browser tab displaying the failed run of the job in the Azure DevOps portal.

   ![LAB06b-Az400_62](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_62.png)

   

9. On the failed job pane in the Azure portal, click the **Job** entry to display its details.

   ![LAB06b-Az400_63](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_63.png)

   ![LAB06b-Az400_64](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_64.png)

10. In the list of job tasks, click the **npm install, build, and test** task to view its output.

    ![LAB06b-Az400_65](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_65.png)

11. Locate the section that lists out failing tests.

    > **Note**: It might not be immediately clear why the tests failed, but all of the history we’ve accrued in the pipeline makes it easy to identify that something from this new pull request is the cause. The next step will be to figure out why “21 + 21” produced “2121” instead of the expected “42”.

12. Close the tab displaying the failed run of the job in the Azure DevOps portal.

### Task 5: Using the broken pull request to improve the project

In this task, you will correct the invalid changes introduced in the pull request created in the previous task.

> **Note**: Return to your original mindset of “project owner”.

1. Return to the browser tab displaying the **Modifying the add function #1** GitHub page and return to the main page listing the content of the forked repo.

   ![LAB06b-Az400_66](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_66.png)

2. At the top of the pane containing the listing of the repo files, **Pull requests** and then click the entry representing the most recent pull request.

3. On the **Modifying the add function #1** GitHub page, click the **File changed** tab and review its content.

   ![LAB06b-Az400_67](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_67.png)

   > **Note**: It appears that the changes were made by someone who didn’t realize that the plus signs before each variable were necessary to coerce those variables to their number representations. By removing them, JavaScript interpreted the middle plus sign as the string concatenation operator, which explains why 21 + 21 = 2121 in the failed test.

4. On the **Modifying the add function #1** GitHub page, click the ellipsis symbol directly under the **Review changes** button and, in the drop-down menu, click **Edit file**.

   ![LAB06b-Az400_68](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_68.png)

   ![LAB06b-Az400_69](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_69.png)

5. Revert the original changes by adding the plus signs in front of the **a** and **b** variables, resulting in ‘add’: function(a,b) { return +a + +b },`. In addition, include a comment on the preceding line stating`// Using + operator to type cast variables as integers in order to prevent string concatenation`.

   ![LAB06b-Az400_70](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_70.png)

   ![LAB06b-Az400_71](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_71.png)

   

6. Scroll to the bottom of the page, replace the default commit message with **Fixing the add function**, ensure that the option to **Commit directly to the addition-cleanup branch** is selected, and click **Commit changes**.

   ![LAB06b-Az400_72](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_72.png)

   ![LAB06b-Az400_73](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_73.png)

   ![LAB06b-Az400_74](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_74.png)

7. On the **Modifying the add function #1** GitHub page, select the **Conversation** tab.

   > **Note**: Azure DevOps will again detect the change and start the build pipeline. Wait for all checks to pass.

8. Once all checks have passed, click **Merge pull request** and then click **Confirm merge**.

   ![LAB06b-Az400_76](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_76.png)

   ![LAB06b-Az400_77](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_77.png)

   ![LAB06b-Az400_78](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_78.png)

   

### Task 6: Adding a build status badge

In this task, you will add a build status badge to your GitHub repo.

> **Note**: An important sign for a quality project is its build status badge. When someone finds a project that has a badge indicating that the project is currently in a successful build state, it’s a sign that the project is maintained effectively.

1. Switch back to the browser tab displaying the **Azure DevOps** portal and use the breadcrumb navigation to navigate to the **Recent** tab of the **Pipelines** pane of the **Pipelines** view.

   ![LAB06b-Az400_79](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_79.png)

2. On the **Recent** tab in the **Recently run pipelines** list, click the entry corresponding to the pipeline you used in this lab.

   ![LAB06b-Az400_80](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_80.png)

   ![LAB06b-Az400_81](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_81.png)

   ![LAB06b-Az400_82](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_82.png)

   ![LAB06b-Az400_83](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_83.png)

   ![LAB06b-Az400_84](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_84.png)

3. On the pipeline pane, click the ellipsis symbol in the upper right corner and, in the dropdown list, select **Status badge**.

   ![LAB06b-Az400_85](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_85.png)

   

   > **Note**: The **Status badge** UI provides a quick and easy way to reference the build status. Often, you’ll want to use the provided URLs in your own dashboards, or you can use the Markdown snippet to add the status badge to locations such as Wiki pages.

4. On the **Status badge** pane, click the **Copy to clipboard** button for **Sample Markdown**.

   ![LAB06b-Az400_87](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_87.png)

5. Switch back to the browser tab displaying the GitHub page displaying the content of the forked repo and, if needed, click the **<> Code** tab.

   ![LAB06b-Az400_88](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_88.png)

6. In the list of repo files, click **README.md** and, on the **master/calculator/README.md** page, in the upper right corner of the pane displaying the file content, click the **Edit this file** icon in the shape of a pencil.

   ![LAB06b-Az400_89](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_89.png)

7. Add an extra line above line 6 and paste into it the content of Clipboard.

   ![LAB06b-Az400_90](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_90.png)

   

8. Scroll to the bottom of the page, replace the default commit message with **Add an Azure Pipelines status badge**, and click **Commit changes**.

   ![LAB06b-Az400_91](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_91.png)

   ![LAB06b-Az400_92](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_92.png)
   
   ![LAB06b-Az400_93](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_93.png)
   
   ![LAB06b-Az400_94](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_94.png)
   
   ![LAB06b-Az400_95](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_95.png)
   
   
   
   ![LAB06b-Az400_96](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_96.png)
   
   ![LAB06b-Az400_97](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_97.png)
   
   ![LAB06b-Az400_98](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_98.png)
   
   ![LAB06b-Az400_99](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_99.png)
   
   ![LAB06b-Az400_100](C:\CFTIC-AZ-400_FPM\Az-400_Practicas_FPM\LAB06b\Evidencia\LAB06b-Az400_100.png)
   
   > **Note**: You now have a dynamic build status badge on your project’s front page that allows everyone to know that you’re effectively managing your project.

#### Review

In this lab, you integrated a GitHub project with Azure DevOps by using the new Azure Pipelines integration from the Marketplace.