# Integrating Your Microsoft Sentinel Environment with Intezer

This repository provides a comprehensive integration solution between your Microsoft Sentinel environment and Intezer's advanced threat detection and response (ADR) platform. By combining the capabilities of both tools, you can enhance your security operations and streamline incident response.

## Playbook Descriptions

### Playbook 1: Update Incident - Intezer Alert Webhook
Assuming you have a running integration with Intezer's ADR solution and Microsoft Defender, this playbook triggers whenever Intezer completes alert processing. It appends a relevant comment to the corresponding incident within Microsoft Sentinel.

### Playbook 2: Submit Intezer Alert - Incident Triggered
This playbook utilizes Microsoft Sentinel incident details to generate an alert within Intezer's system.

### Playbook 3: Submit Intezer Scan File Hash - Incident Triggered
This playbook retrieves file hashes from the Microsoft Sentinel incident, forwards them to Intezer Analyze, and appends a comment containing the resulting verdict back to the incident.

### Playbook 4: Submit Intezer Scan URL - Incident Triggered
This playbook retrieves URLs from the Microsoft Sentinel incident, forwards them to Intezer Analyze, and appends a comment containing the resulting verdict back to the incident.

## Quick Deployment
Before deploying, please review the [deployment prerequisites](#deployment-prerequisites).

Click on the "Deploy to Azure" link and complete the sections with the data extracted in the prerequisites:  
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fintezer%2Fmicrosoft-sentinel-integration%2Fmain%2Fplaybooks%2Fazuredeploy.json)
![Image Alt Text](.github/sentinel-11.png)

After deployment, please review the [deployment postrequisites](#deployment-postrequisites).

### Deployment Prerequisites
1. Extracting Your Microsoft Sentinel Workspace Name and Workspace ID:
   - Navigate to **Microsoft Sentinel** in the Azure portal:  
     ![Image Alt Text](.github/sentinel-1.png)
   - Select your Microsoft Sentinel workspace:  
     ![Image Alt Text](.github/sentinel-2.png)
   - Go to **Settings**:  
     ![Image Alt Text](.github/sentinel-3.png)
   - Select **Workspace Settings**:  
     ![Image Alt Text](.github/sentinel-4.png)
   - Under the **Essentials** section, locate your workspace name and workspace ID. Keep this information for later use:  
     ![Image Alt Text](.github/sentinel-5.png)

2. Setting Up Azure Vault for Holding Intezer API Key:
   - Go to **Key Vaults** in the Azure portal:  
     ![Image Alt Text](.github/sentinel-6.png)
   - Create a new vault, choosing a unique name for the **Key Vault Name** and keeping it for later use:  
     ![Image Alt Text](.github/sentinel-7.png)
     ![Image Alt Text](.github/sentinel-8.png)
   - Access your created vault and create a new secret named **intezer-sentinel-api-key** to hold your Intezer API key:  
     ![Image Alt Text](.github/sentinel-9.png)
     ![Image Alt Text](.github/sentinel-10.png)

### Deployment Postrequisites
After submitting the custom deployment, you need to perform some actions in your Azure environment.

1. Grant Permissions to Microsoft Sentinel Created Playbooks:
   - Navigate to **Microsoft Sentinel** in the Azure portal:  
     ![Image Alt Text](.github/sentinel-1.png)
   - Select your Microsoft Sentinel workspace:  
     ![Image Alt Text](.github/sentinel-2.png)
   - Go to **Settings**:  
     ![Image Alt Text](.github/sentinel-3.png)
   - Select **Workspace Settings**:  
     ![Image Alt Text](.github/sentinel-4.png)
   - In the left menu, choose **Access Control (IAM)**:  
     ![Image Alt Text](.github/sentinel-12.png)
   - Click **Add**, then **Add Role Assignment**:  
     ![Image Alt Text](.github/sentinel-13.png)
   - Under the Role tab, in the Job Function Rules tab, search and select **Microsoft Sentinel Responder**, then click Next:  
     ![Image Alt Text](.github/sentinel-14.png)
   - Click **+ Select Members** and add the four logic app playbooks created during the deployment process, then click **Review + Assign**:  
     ![Image Alt Text](.github/sentinel-15.png)
     ![Image Alt Text](.github/sentinel-16.png)

2. Grant Permissions to the Created Azure Vault:
   - During the prerequisites stage, you created an Azure vault that holds the "intezer-sentinel-api-key." After deploying logic app playbooks, ensure that the vault has permissions to read secrets during a run.
   - Go to **Key Vaults** in the Azure portal:  
     ![Image Alt Text](.github/sentinel-6.png)
   - Select the vault you created in the prerequisites; for example, "my-unique-vault-intezer." Choose **Access Control (IAM)**:  
     ![Image Alt Text](.github/sentinel-17.png)
   - Click **Add**, then **Add Role Assignment**:  
     ![Image Alt Text](.github/sentinel-18.png)
   - Under the Role tab, in the Job Function Rules tab, search and select **Key Vault Reader**, then click Next:  
     ![Image Alt Text](.github/sentinel-19.png)
   - Click **+ Select Members** and add the four logic app playbooks created during the deployment process, then click **Review + Assign**:  
     ![Image Alt Text](.github/sentinel-20.png)

3. Enable the Monitor Logs API Connection:
   - Select **API Connections** in the Azure portal:  
     ![Image Alt Text](.github/sentinel-21.png)
   - Search for and select **monitorlogs-intezer-connection** (it might be in an error status):  
     ![Image Alt Text](.github/sentinel-22.png)
   - In the left menu bar, select **Edit API Connection**:  
     ![Image Alt Text](.github/sentinel-23.png)
   - Authorize using your account and click Save:  
     ![Image Alt Text](.github/sentinel-24.png)
