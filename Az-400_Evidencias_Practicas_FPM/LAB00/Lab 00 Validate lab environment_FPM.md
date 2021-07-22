# Lab 00: Validate lab environment

# Student lab manual

## Instructions

1. Get a new **Azure Pass promocode** (valid for 30 days) from the instructor or other source.

2. Use a private browser session to get a new **Microsoft Account (MSA)** at https://account.microsoft.com or use an existing one.

3. Using the same browser session, go to https://www.microsoftazurepass.com to redeem your Azure Pass using your Microsoft Account (MSA). Fore details, see [Redeem a Microsoft Azure Pass](https://www.microsoftazurepass.com/Home/HowTo?Length=5). Follow the instructions for redemption.

   ![LAB00-Az400_01](Evidencia/LAB00-Az400_01.png)

   ![LAB00-Az400_02](Evidencia/LAB00-Az400_02.png)

   ![LAB00-Az400_03](Evidencia/LAB00-Az400_03.png)

4. Using the same browser session, go to https://portal.azure.com, then search at the top of the portal screen for **Azure DevOps**. In the resulting page, click **Azure DevOps organizations**.

   ![LAB00-Az400_04](Evidencia/LAB00-Az400_04.png)

5. Next, click on the link labelled **My Azure DevOps Organizations** (or navigate directly to https://aex.dev.azure.com).

   ![LAB00-Az400_05](Evidencia/LAB00-Az400_05.png)

   ![LAB00-Az400_06](Evidencia/LAB00-Az400_06.png)

6. In the drop-down box on the left, choose **Default Directory**, instead of “Microsoft Account”.

7. If prompted (*“We need a few more details”*), provide your name, e-mail address, and location and click **Continue**.

   ![LAB00-Az400_07](Evidencia/LAB00-Az400_07.png)

8. Back at https://aex.dev.azure.com with **Default Directory** selected click the blue button **Create new organization**.

   ![LAB00-Az400_09](Evidencia/LAB00-Az400_09.png)

9. Accept the *Terms of Service* by clicking **Continue**.

   ![LAB00-Az400_10](Evidencia/LAB00-Az400_10.png)

10. If prompted (*“Almost done”*), leave the name for the Azure DevOps organization at default (it needs to be a globally unique name) and pick a hosting location close to you from the list.

    ![LAB00-Az400_11](Evidencia/LAB00-Az400_11.png)

    ![LAB00-Az400_12](Evidencia/LAB00-Az400_12.png)

11. Once the newly created organization opens in **Azure DevOps**, click **Organization settings** in the bottom left corner.

    ![LAB00-Az400_13](Evidencia/LAB00-Az400_13.png)

12. At the **Organization settings** screen click **Billing** (opening this screen takes a few seconds).

    ![LAB00-Az400_14](Evidencia/LAB00-Az400_14.png)

    ![LAB00-Az400_15](Evidencia/LAB00-Az400_15.png)

13. Click **Setup billing** and on the right-hand side of the screen select the **Azure Pass - Sponsorship** subscription and click **Save** to link the subscription with the organization.

    ![LAB00-Az400_16](Evidencia/LAB00-Az400_16.png)

    ![LAB00-Az400_17](Evidencia/LAB00-Az400_17.png)

    ![LAB00-Az400_18](Evidencia/LAB00-Az400_18.png)

14. Once the screen shows the linked Azure Subscription ID at the top, change the number of **Paid parallel jobs** for **MS Hosted CI/CD** from 0 to **1**. Then click the **SAVE** button at the bottom.

    ![LAB00-Az400_19](Evidencia/LAB00-Az400_19.png)

    ![LAB00-Az400_20](Evidencia/LAB00-Az400_20.png)

    ![LAB00-Az400_21](Evidencia/LAB00-Az400_21.png)

    ![LAB00-Az400_22](Evidencia/LAB00-Az400_22.png)

15. **Wait at least 3 hours before using the CI/CD capabilities** so that the new settings are reflected in the back end. Otherwise, you will still see the message *“This agent is not running because you have reached the maximum number of requests…”*.

16. OPTIONAL: you can validate this new setting has been successfully applied by creating and triggering a build pipeline. To do this either talk to the instructor or create a demo project into the newly created organization with billing enabled, using https://azuredevopsdemogenerator.azurewebsites.net.