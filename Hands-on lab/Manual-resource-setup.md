# Appendix A: Manual lab environment setup

This appendix provides the steps to manually provision and configure the resources created by the ARM template used before the hands-on lab guide.

**Contents**:

- [Appendix A: Manual lab environment setup](#appendix-a-manual-lab-environment-setup)
  - [Task 1: Create an Azure Storage account](#task-1-create-an-azure-storage-account)
  - [Task 2: Create the LabVM](#task-2-create-the-labvm)
  - [Task 3: Create SQL Server 2008 R2 virtual machine](#task-3-create-sql-server-2008-r2-virtual-machine)
  - [Task 4: Provision an Azure SQL Database](#task-4-provision-an-azure-sql-database)
  - [Task 5: Create Azure Database Migration Service](#task-5-create-azure-database-migration-service)
  - [Task 6: Provision a Web App](#task-6-provision-a-web-app)
  - [Task 7: Provision an API App](#task-7-provision-an-api-app)
  - [Task 8: Provision a Function App](#task-8-provision-a-function-app)
  - [Task 9: Provision API Management](#task-9-provision-api-management)
  - [Task 10: Create Cognitive Services account](#task-10-create-cognitive-services-account)
  - [Task 11: Create an Azure Key Vault](#task-11-create-an-azure-key-vault)
  - [Task 12: Connect to the Lab VM](#task-12-connect-to-the-lab-vm)
  - [Task 13: Install required software on the LabVM](#task-13-install-required-software-on-the-labvm)
  - [Task 14: Connect to SqlServer2008 VM](#task-14-connect-to-sqlserver2008-vm)
  - [Task 15: Install required software on the SqlServer2008 VM](#task-15-install-required-software-on-the-sqlserver2008-vm)

> **IMPORTANT**: Many Azure resources require globally unique names. Throughout these steps you will see the word "SUFFIX" as part of resource names. You should replace this with your Microsoft alias, initials, or another value to ensure resources are uniquely named.

## Task 1: Create an Azure Storage account

In this task, you will provision an Azure Storage account, which will be used for storing policy documents, as well as vulnerability assessments performed using SQL Advanced Data Security.

1. In the [Azure portal](https://portal.azure.com/), select **+Create a resource**, enter "storage account" into the Search the Marketplace box and select **Storage account** from the results, and then select **Create**.

    ![+Create a resource is selected in the Azure navigation pane, and "storage account" is entered into the Search the Marketplace box. Storage account is selected in the results.](./media/create-resource-storage-account.png "Create Storage account")

2. On the Create storage account **Basics** tab, enter the following:

    - Project Details:

        - **Subscription**: Select the subscription you are using for this hands-on lab.
        - **Resource Group**: Select the hands-on-lab-SUFFIX resource group from the list of existing resource groups.

    - Instance Details:

        - **Storage account name**: Enter contosostoreSUFFIX.
        - **Location**: Select the location you are using for resources in this hands-on lab.
        - **Performance**: Choose **Standard**.
        - **Account kind**: Select **StorageV2 (general purpose v2)**.
        - **Replication**: Select **Locally-redundant storage (LRS)**.
        - **Access tier**: Choose **Hot**.

    ![On the Create storage account blade, the values specified above are entered into the appropriate fields.](media/storage-create-account.png "Create storage account")

3. Select **Review + create**.

4. On the **Review + create** blade, ensure the Validation passed message is displayed and then select **Create**.

## Task 2: Create the LabVM

In this task, you will provision a virtual machine (VM) in Azure. The VM image used will have Visual Studio Community 2019 installed.

1. In the [Azure portal](https://portal.azure.com/), select **+Create a resource**, enter "visual studio 2019" into the Search the Marketplace box and then select **Visual Studio 2019 Latest** from the results.

    ![+Create a resource is selected in the Azure navigation pane, and "visual studio 2019" is entered into the Search the Marketplace box. Visual Studio 2019 latest is selected in the results.](./media/create-resource-visual-studio-vm.png "Visual Studio 2019 Latest")

2. On the Visual Studio 2019 Latest blade, select **Visual Studio 2019 Community (latest release) on Windows Server 2019 (x64)** from the Select a software plan drop down list, and then select **Create**.

    ![On the Visual Studio 2019 Latest blade, Visual Studio 2019 Community (latest release) on Windows Server 2019 (x64) is highlighted in the Select a software plan drop down list.](media/visual-studio-create.png "Visual Studio 2019 Latest")

3. On the Create a virtual machine **Basics** tab, set the following configuration:

    - Project Details:

        - **Subscription**: Select the subscription you are using for this hands-on lab.
        - **Resource Group**: Select the **hands-on-lab-SUFFIX** resource group from the list of existing resource groups.

    - Instance Details:

        - **Virtual machine name**: Enter LabVM.
        - **Region**: Select the region you are using for resources in this hands-on lab.
        - **Availability options**: Select no infrastructure redundancy required.
        - **Image**: Leave Visual Studio 2019 Community (latest release) on Windows Server 2019 (x64) selected.
        - **Size**: Select **Change size**, and select Standard D2s v3 from the list and then select **Accept**.

    - Administrator Account:

        - **Username**: Enter **demouser**.
        - **Password**: Enter **Password.1!!**

    - Inbound Port Rules:

        - **Public inbound ports**: Choose Allow selected ports.
        - **Select inbound ports**: Select RDP (3389) in the list.

    ![Screenshot of the Basics tab, with fields set to the previously mentioned settings.](media/lab-virtual-machine-basics-tab.png "Create a virtual machine Basics tab")

    > **Note**: The remaining tabs can be skipped, and default values will be used.

4. Select **Review + create** to validate the configuration.

5. On the **Review + create** tab, ensure the Validation passed message is displayed, and then select **Create** to provision the virtual machine.

    ![The Review + create tab is displayed, with a Validation passed message.](media/lab-virtual-machine-review-create-tab.png "Create a virtual machine Review + create tab")

6. It will take approximately 10 minutes for the VM to finish provisioning. You can move on to the next task while you wait.

## Task 3: Create SQL Server 2008 R2 virtual machine

In this task, you will provision another virtual machine (VM) in Azure which will host your "on-premises" instance of SQL Server 2008 R2. The VM will use the SQL Server 2008 R2 SP3 Standard on Windows Server 2008 R2 image.

> **Note**:  An older version of Windows Server is being used because SQL Server 2008 R2 is not supported on Windows Server 2016.

1. In the [Azure portal](https://portal.azure.com/), select **+Create a resource**, enter "SQL Server 2008R2SP3 on Windows Server 2008R2" into the Search the Marketplace box.

2. On the **SQL Server 2008 R2 SP3 Standard on Windows Server 2008 R2** blade, select **SQL Server R2 SP3 Standard on Windows Server 2008 R2** for the software plan and then select **Create**.

    ![The SQL Server 2008 R2 SP3 on Windows Server 2008 R2 blade is displayed with the standard edition selected for the software plan, and the Create button highlighted.](media/create-resource-sql-server-2008-r2.png "Create SQL Server 2008 R2 Resource")

3. On the Create a virtual machine Basics tab, set the following configuration:

   - Project Details:

     - **Subscription**: Select the subscription you are using for this hands-on lab.
     - **Resource Group**: Select the hands-on-lab-SUFFIX resource group from the list of existing resource groups.

   - Instance Details:

     - **Virtual machine name**: Enter SqlServer2008.
     - **Region**: Select the region you are using for resources in this hands-on lab.
     - **Availability options**: Select no infrastructure redundancy required.
     - **Image**: Leave SQL Server 2008 R2 SP3 Standard on Windows Server 2008 R2 selected.
     - **Size**: Accept the default size, Standard DS12 v2.

   - Administrator Account:

     - **Username**: Enter **demouser**.
     - **Password**: Enter **Password.1!!**

   - Inbound Port Rules:

     - **Public inbound ports**: Choose Allow selected ports.
     - **Select inbound ports**: Select RDP (3389) in the list.

    ![Screenshot of the Basics tab, with fields set to the previously mentioned settings.](media/sql-server-2008-r2-vm-basics-tab.png "Create a virtual machine Basics tab")

4. Select the **SQL Server settings** tab from the top menu. The default values will be used for Disks, Networking, Management and Advanced, so you don't need to do anything on those tabs.

    ![The SQL Server settings tab is highlighted and selected in the Create a virtual machine configuration tabs list.](media/sql-server-create-vm-sql-settings-tab.png "Create a virtual machine configuration tabs")

5. On the **SQL Server settings** tab, set the following properties:

   - Security & Networking:

     - **SQL connectivity**: Select Public (Internet).
     - **Port**: Leave set to 1433.

   - SQL Authentication:

     - **SQL Authentication**: Select Enable.
     - **Login name**: Enter demouser.
     - **Password**: Enter **Password.1!!**

     ![The previously specified values are entered into the SQL Server Settings blade.](media/sql-server-create-vm-sql-settings.png "SQL Server Settings")

6. Select **Review + create** to review the VM configuration.

7. On the **Review + create** tab, ensure the Validation passed message is displayed, and then select **Create** to provision the virtual machine.

    ![The Review + create tab is displayed, with a Validation passed message.](media/sql-server-2008-r2-vm-review-create-tab.png "Create a virtual machine Review + create tab")

8. It may take 10+ minutes for the virtual machine to complete provisioning. You can move on to the next task while waiting for the SqlServer2008 VM to provision.

## Task 4: Provision an Azure SQL Database

In this task, you will provision an Azure SQL Database (Azure SQL DB).

1. In the [Azure portal](https://portal.azure.com/), select **+Create a resource**, enter "sql database" into the Search the Marketplace box, select **SQL Database** from the results, and then select **Create**.

    ![+Create a resource is selected in the Azure navigation pane, and "sql database" is entered into the Search the Marketplace box. SQL Database is selected in the results.](media/create-resource-azure-sql-database.png "Create Azure SQL Database")

2. On the Create SQL Database **Basics** tab, enter the following:

    - Project Details:

        - **Subscription**: Select the subscription you are using for this hands-on lab.
        - **Resource Group**: Select the hands-on-lab-SUFFIX resource group from the list of existing resource groups.

    - Database Details:

        - **Database name**: Enter ContosoInsurance.
        - **Server**: Select **Create new**, and on the New server blade enter the following:
          - **Server name**: Enter contosoinsuranceSUFFIX.
          - **Server admin login**: Enter demouser.
          - **Password**: Enter Password.1!!
          - **Location**: Select the region you are using for resources in this hands-on lab.
          - **Allow Azure services to access server**: Check this box.
          - Select **Select**.
        - **Want to use SQL elastic pool**: Choose No.
        - **Compute + storage**: Accept the default, SO with 10 DTUs and 250 GB storage.

    ![The Create SQL Database Basics tab is displayed, with the values specified above entered into the appropriate fields.](media/create-sql-database-basics-tab.png "Create SQL Database")

3. Select **Next: Additional settings**.

4. On the **Additional settings** tab, **Enable Advanced Data Security** by selecting **Start free trial**.

    ![On the Additional settings tab, Enable Advanced Data Security is highlighted and Start free trial is selected.](media/create-sql-database-additional-settings-tab.png "Create SQL Database")

5. Select **Review + create**.

6. On the **Review + create** tab, select **Create** to provision the SQL Database.

## Task 5: Create Azure Database Migration Service

In this task, you will provision an instance of the Azure Database Migration Service (DMS).

1. In the [Azure portal](https://portal.azure.com/), select **+Create a resource**, enter "database migration" into the Search the Marketplace box, select **Azure Database Migration Service** from the results, and select **Create**.

    ![+Create a resource is selected in the Azure navigation pane, and "database migration" is entered into the Search the Marketplace box. Azure Database Migration Service is selected in the results.](media/create-resource-azure-database-migration-service.png "Create Azure Database Migration Service")

2. On the Create Migration Service blade, enter the following:

    - **Service Name**: Enter contoso-dms.
    - **Subscription**: Select the subscription you are using for this hands-on lab.

        > **Note**: If you see the message `Your subscription doesn't have proper access to Microsoft.DataMigration`, refresh the browser window before proceeding. If the message persists, verify you successfully registered the resource provider, and then you can safely ignore this message.

    - **Resource Group**: Select the hands-on-lab-SUFFIX resource group from the list of existing resource groups.
    - **Location**: Select the location you are using for resources in this hands-on lab.
    - **Virtual network**: Select the **hands-on-lab-SUFFIX-vnet/default** virtual network, and then select **OK**. This will place the DMS instance into the same VNet as your LabVM and SqlServer2008 virtual machines.
    - **Pricing tier**: Select Premium: 4 vCores.

    ![The Create Migration Service blade is displayed, with the values specified above entered into the appropriate fields.](media/create-migration-service.png "Create Migration Service")

3. Select **Create**.

4. It can take 15 minutes to deploy the Azure Data Migration Service. You can move on to the next task while you wait.

## Task 6: Provision a Web App

In this task, you will provision an App Service (Web app), which will be used for hosting the Contoso Insurance web application.

1. In the [Azure portal](https://portal.azure.com/), select **+Create a resource**, enter "web app" into the Search the Marketplace box, select **Web App** from the results.

    ![+Create a resource is selected in the Azure navigation pane, and "web app" is entered into the Search the Marketplace box. Web App is selected in the results.](media/create-resource-web-app.png "Create Web App")

2. On the Web App blade, select **Create**.

    ![On the Web App blade, the Create button is highlighted.](media/create-web-app.png "Create Web App")

3. On the Create Web App **Basics** tab, set the following configuration:

    - Project Details:

        - **Subscription**: Select the subscription you are using for this hands-on lab.
        - **Resource Group**: Select the hands-on-lab-SUFFIX resource group from the list of existing resource groups.

    - Instance Details:

        - **Name**: Enter contosoinswebSUFFIX.
        - **Publish**: Select Code.
        - **Runtime stack**: Select .NET Core 2.2.
        - **Operating System**: Select Windows.
        - **Location**: Select the location you are using for resources in this hands-on lab.

    - App Service Plan:

        - **Plan**: Select **Create new** and enter **hands-on-lab-asp** for the name.
        - **Sku and size**: Accept the default value of Standard S1.

    ![The values specified above are entered into the appropriate fields in the Create Web App Basics tab.](media/create-web-app-basics-tab.png "Create Web App")

4. Select **Next: Monitoring**.

5. On the **Monitoring** tab, select **No** next to Enable Application Insights.

    ![No is selected next to Enable Application Insights on the Monitoring tab.](media/create-web-app-monitoring-tab.png "Create Web App")

6. Select **Review and create**.

7. On the **Review and create** tab, select **Create**.

8. It will take a few minutes for the Web App creation to complete. You can move on to the next task while you wait.

## Task 7: Provision an API App

In this task, you will provision an App Service (API App), which will be used for hosting the Contoso Insurance APIs.

1. In the [Azure portal](https://portal.azure.com/), select **+Create a resource**, enter "api app" into the Search the Marketplace box, select **API App** from the results.

    ![+Create a resource is selected in the Azure navigation pane, and "api app" is entered into the Search the Marketplace box. API App is selected in the results.](media/create-resource-api-app.png "Create Web App")

2. On the API App blade, select **Create**.

3. On the API App Create blade, enter the following:

    - **App name**: Enter contosoinsapiSUFFIX.
    - **Subscription**: Select the subscription you are using for this hands-on lab.
    - **Resource Group**: Choose Use exiting and select the hands-on-lab-SUFFIX resource group from the list of existing resource groups.
    - **App Service plan/Location**: Select the hands-on-lab-asp App Service plan.
    - **Application Insights**: Select **Disabled**.

    ![On the API App Create blade, the values specified above are entered into the appropriate fields.](media/create-api-app-basics-tab.png "Create API App")

4. Select **Create**.

## Task 8: Provision a Function App

In this task, you will provision Function App, which will be used for retrieving PDF documents from Azure Storage.

1. In the [Azure portal](https://portal.azure.com/), select **+Create a resource**, enter "function app" into the Search the Marketplace box, select **Function App** from the results.

    ![+Create a resource is selected in the Azure navigation pane, and "function app" is entered into the Search the Marketplace box. Function App is selected in the results.](media/create-resource-function-app.png "Create Web App")

2. On the Function App blade, select **Create**.

3. On the Function App Create blade, enter the following:

    - **App name**: Enter contosoinsfuncSUFFIX.
    - **Subscription**: Select the subscription you are using for this hands-on lab.
    - **Resource Group**: Choose Use exiting and select the hands-on-lab-SUFFIX resource group from the list of existing resource groups.
    - **OS**: Select Windows.
    - **Hosting Plan**: Select Consumption Plan.
    - **Location**: Select the location you are using for resources in this hands-on lab.
    - **Runtime Stack**: Select .NET.
    - **Storage**: Choose Create new and accept the default name.
    - **Application Insights**: Select **Disabled**.

    ![On the Function App Create blade, the values specified above are entered into the associated fields.](media/function-app-create.png "Create Function App")

4. Select **Create**.

## Task 9: Provision API Management

In this task, you will provision API Management, which will be used for managing the Contoso APIs.

1. In the [Azure portal](https://portal.azure.com/), select **+Create a resource**, enter "api management" into the Search the Marketplace box, select **API Management** from the results.

    ![+Create a resource is selected in the Azure navigation pane, and "api management" is entered into the Search the Marketplace box. API Management is selected in the results.](media/create-resource-api-management.png "Create Web App")

2. On the API Management blade, select **Create**.

3. On the API Management service blade, enter the following:

    - **Name**: Enter contosoinsapimSUFFIX.
    - **Subscription**: Select the subscription you are using for this hands-on lab.
    - **Resource Group**: Select the hands-on-lab-SUFFIX resource group from the list of existing resource groups.
    - **Location**: Select the location you are using for resources in this hands-on lab.
    - **Organization name**: Enter Contoso Insurance.
    - **Administrator email**: Enter an email add that can receive API Management admin notifications.
    - **Pricing tier**: Select Developer (No SLA).
    - **Enable Application Insights**: Uncheck this box.

    ![The values specified above are entered into the API Management service blade.](media/api-management-service-create.png "Create API Management service")

4. Select **Create**.

5. It will take around 30 minutes for API Management to finish provisioning. You can move on to the next task while you wait.

## Task 10: Create Cognitive Services account

In this task, you will create a Cognitive Services account.

1. In the [Azure portal](https://portal.azure.com/), select **+Create a resource**, enter "cognitive services" into the Search the Marketplace box, select **Cognitive Services** from the results.

    ![+Create a resource is selected in the Azure navigation pane, and "cognitive services" is entered into the Search the Marketplace box. Cognitive Services is selected in the results.](media/create-resource-cognitive-services.png "Create Cognitive Services account")

2. On the Cognitive Services blade, select **Create**.

3. On the Cognitive Services Create blade, enter the following:

    - **Name**: Enter contoso-cog-services.
    - **Subscription**: Select the subscription you are using for this hands-on lab.
    - **Location**: Select the region you are using for resources in this hands-on lab.
    - **Pricing tier**: Select S0.
    - **Resource Group**: Select the hands-on-lab-SUFFIX resource group from the list of existing resource groups.
    - Check the box to confirm you have read and understood the notice.

    ![The values specified above are entered into the Create blade.](media/cognitive-services-create.png "Create Cognitive Services")

4. Select **Create**.

## Task 11: Create an Azure Key Vault

In this task, you will provision an Azure Key Vault, which will be used for securing application secrets.

1. In the [Azure portal](https://portal.azure.com/), select **+Create a resource**, enter "key vault" into the Search the Marketplace box, select **Key Vault** from the results.

    ![+Create a resource is selected in the Azure navigation pane, and "key vault" is entered into the Search the Marketplace box. Key Vault is selected in the results.](media/create-resource-key-vault.png "Create Key Vault")

2. On the Key Vault blade, select **Create**.

3. On the Create Key Vault Basics tab, enter the following:

    - Project Details:

      - **Subscription**: Select the subscription you are using for this hands-on lab.
      - **Resource Group**: Select the hands-on-lab-SUFFIX resource group from the list of existing resource groups.

    - Instance Details:

      - **Key vault name**: Enter contosoinskvSUFFIX.
      - **Region**: Select the region you are using for resources in this hands-on lab.
      - **Pricing tier**: Select Standard.

    ![The values specified above are entered into the Create Key Vault blade.](media/key-vault-create-basics-tab.png "Create Key Vault")

4. Select **Review + create**. The default values for Access policy and Virtual network are used, so these tabs can be skipped.

5. On the **Review + create** tab, select **Create**.

## Task 12: Connect to the Lab VM

In this task, you will create an RDP connection to your Lab virtual machine (VM), and disable Internet Explorer Enhanced Security Configuration.

1. In the [Azure portal](https://portal.azure.com), select **Resource groups** in the Azure navigation pane, and select the hands-on-lab-SUFFIX resource group from the list.

    ![Resource groups is selected in the Azure navigation pane and the "hands-on-lab-SUFFIX" resource group is highlighted.](./media/resource-groups.png "Resource groups list")

2. In the list of resources for your resource group, select LabVM.

    ![The list of resources in the hands-on-lab-SUFFIX resource group are displayed, and LabVM is highlighted.](./media/resource-group-resources-labvm.png "LabVM in resource group list")

3. On your LabVM blade, select **Connect** from the top menu.

    ![The LabVM blade is displayed, with the Connect button highlighted in the top menu.](./media/connect-labvm.png "Connect to Lab VM")

4. On the Connect to virtual machine blade, select **Download RDP File**, then open the downloaded RDP file.

    ![The Connect to virtual machine blade is displayed, and the Download RDP File button is highlighted.](./media/connect-to-virtual-machine.png "Connect to virtual machine")

5. Select **Connect** on the Remote Desktop Connection dialog.

    ![In the Remote Desktop Connection Dialog Box, the Connect button is highlighted.](./media/remote-desktop-connection.png "Remote Desktop Connection dialog")

6. Enter the following credentials when prompted, and then select **OK**:

    - **User name**: demouser
    - **Password**: Password.1!!

    ![The credentials specified above are entered into the Enter your credentials dialog.](media/rdc-credentials.png "Enter your credentials")

7. Select **Yes** to connect, if prompted that the identity of the remote computer cannot be verified.

    ![In the Remote Desktop Connection dialog box, a warning states that the identity of the remote computer cannot be verified, and asks if you want to continue anyway. At the bottom, the Yes button is circled.](./media/remote-desktop-connection-identity-verification-labvm.png "Remote Desktop Connection dialog")

8. Once logged in, launch the **Server Manager**. This should start automatically, but you can access it via the Start menu if it does not.

9. Select **Local Server**, then select **On** next to **IE Enhanced Security Configuration**.

    ![Screenshot of the Server Manager. In the left pane, Local Server is selected. In the right, Properties (For LabVM) pane, the IE Enhanced Security Configuration, which is set to On, is highlighted.](./media/windows-server-manager-ie-enhanced-security-configuration.png "Server Manager")

10. In the Internet Explorer Enhanced Security Configuration dialog, select **Off** under both Administrators and Users, and then select **OK**.

    ![Screenshot of the Internet Explorer Enhanced Security Configuration dialog box, with Administrators set to Off.](./media/internet-explorer-enhanced-security-configuration-dialog.png "Internet Explorer Enhanced Security Configuration dialog box")

11. Close the Server Manager, but leave the connection to the LabVM open for the next task.

## Task 13: Install required software on the LabVM

In this task, you will install SQL Server Management Studio (SSMS) on the LabVM.

1. First, you will install SSMS on the LabVM. Open a web browser on your LabVM, navigate to <https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms> and then select the **Download SQL Server Management Studio 18.x** link to download the latest version of SSMS.

    ![The Download SQL Server Management Studio 18.x link is highlighted on the page specified above.](media/download-ssms.png "Download SSMS")

    > **Note**: Versions change frequently, so if the version number you see does not match the screenshot, just download and install the most recent version.

2. Run the downloaded installer.

3. On the Welcome screen, select **Install** to begin the installation.

    ![The Install button is highlighted on the SSMS installation welcome screen.](media/ssms-install.png "Install SSMS")

4. Select **Close** when the installation completes.

    ![The Close button is highlighted on the SSMS Setup Completed dialog.](media/ssms-install-close.png "Setup completed")

## Task 14: Connect to SqlServer2008 VM

In this task, you will open an RDP connection to the SqlServer2008 VM, disable Internet Explorer Enhanced Security Configuration, and add a firewall rule to open port 1433 to inbound TCP traffic. You will also install Data Migration Assistant (DMA).

1. As you did for the LabVM, navigate to the SqlServer2008 VM blade in the Azure portal, select **Overview** from the left-hand menu, and then select **Connect** on the top menu.

    ![The SqlServer2008 VM blade is displayed, with the Connect button highlighted in the top menu.](./media/connect-sqlserver2008.png "Connect to SqlServer2008 VM")

2. On the Connect to virtual machine blade, select **Download RDP File**, then open the downloaded RDP file.

3. Select **Connect** on the Remote Desktop Connection dialog.

    ![In the Remote Desktop Connection Dialog Box, the Connect button is highlighted.](./media/remote-desktop-connection-sql-2008.png "Remote Desktop Connection dialog")

4. Enter the following credentials when prompted, and then select **OK**:

    - **User name**: demouser
    - **Password**: Password.1!!

    ![The credentials specified above are entered into the Enter your credentials dialog.](media/rdc-credentials-sql-2008.png "Enter your credentials")

5. Select **Yes** to connect, if prompted that the identity of the remote computer cannot be verified.

    ![In the Remote Desktop Connection dialog box, a warning states that the identity of the remote computer cannot be verified, and asks if you want to continue anyway. At the bottom, the Yes button is circled.](./media/remote-desktop-connection-identity-verification-sqlserver2008.png "Remote Desktop Connection dialog")

6. Once logged in, launch the **Server Manager**. This should start automatically, but you can access it via the Start menu if it does not.

7. On the **Server Manager** view, select **Configure IE ESC** under Security Information.

    ![Screenshot of the Server Manager. In the left pane, Local Server is selected. In the right, Properties (For LabVM) pane, the IE Enhanced Security Configuration, which is set to On, is highlighted.](./media/windows-server-2008-manager-ie-enhanced-security-configuration.png "Server Manager")

8. In the Internet Explorer Enhanced Security Configuration dialog, select **Off** under both Administrators and Users, and then select **OK**.

    ![Screenshot of the Internet Explorer Enhanced Security Configuration dialog box, with Administrators set to Off.](./media/2008-internet-explorer-enhanced-security-configuration-dialog.png "Internet Explorer Enhanced Security Configuration dialog box")

## Task 15: Install required software on the SqlServer2008 VM

In this task, you will install the Microsoft Data Migration Assistant (DMA) on the SqlServer2008 VM.

1. Next, you will install the Microsoft Data Migration Assistant v4.x by navigating to <https://www.microsoft.com/en-us/download/details.aspx?id=53595> in a web browser on the SqlServer2008 VM, and then selecting the **Download** button.

    ![The Download button is highlighted on the Data Migration Assistant download page.](media/dma-download.png "Download Data Migration Assistant")

    > **Note**: Versions change frequently, so if the version number you see does not match the screenshot, download and install the most recent version.

2. Run the downloaded installer.

3. Select **Next** on each of the screens, accepting to the license terms and privacy policy in the process.

4. Select **Install** on the Privacy Policy screen to begin the installation.

5. On the final screen, select **Finish** to close the installer.

    ![The Finish button is selected on the Microsoft Data Migration Assistant Setup dialog.](./media/data-migration-assistant-setup-finish.png "Run the Microsoft Data Migration Assistant")