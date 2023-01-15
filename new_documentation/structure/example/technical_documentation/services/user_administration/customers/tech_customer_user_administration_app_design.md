***
*Name:* Customer User Administration - Power App Design

*Version:* 0.1.0

*Last updated:* 12/01/2022

*Author:* Sebastian Grigoleit

*Required Knowledge:* Azure automation account, Microsoft Flow, Power Apps, PowerShell

*Target audience:* IT Admins, IT Support, DevOps Engineer

*Tags:* Power Apps, Technical design, Customer user administration
***

## Table of Contents
1. [Purpose](#Purpose)
2. [Scope](#Scope)
3. [Definitions](#Definitions)
4. [Procedures](#Procedures)
5. [Resources](#Resources)

# Purpose
The purpose of this document is to provide walkthrough of how the Power App: [Customer User Administration](https://apps.powerapps.com/play/e/default-248b066d-c6fc-4b1a-afba-4138e54e2689/a/66961346-8949-4940-a5f2-9bda7d02ed1f?tenantId=248b066d-c6fc-4b1a-afba-4138e54e2689) is designed. 

# Scope
The scope of this document is limited to the technical design of the "Customer User Administration" app, along with it's dependencies like, Azure automation accounts, Microsoft Flow, Sharepoint Online, and PowerShell. This document will not cover the use of the app, nor the process for managing users with it.

# Definitions
Power App - Refers to Microsoft Power Apps

# Procedures
We will first list and reference all resources used to power the functions of the Power App. Then we will cover the functionality and how it's all connected.

## 1. The Power App 
[Customer User Administration](https://apps.powerapps.com/play/e/default-248b066d-c6fc-4b1a-afba-4138e54e2689/a/66961346-8949-4940-a5f2-9bda7d02ed1f?tenantId=248b066d-c6fc-4b1a-afba-4138e54e2689)

## 2. Azure Automation
**Automation Account:** 

[NISHQ-Management](https://portal.azure.com/#@NordicInsuranceSoftware.onmicrosoft.com/resource/subscriptions/ecd44859-06e3-4231-9db8-c6d26070358f/resourceGroups/NISHQ-Management/providers/Microsoft.Automation/automationAccounts/NISHQ-Management/overview)

**Runbooks:**

- Create Customer User [here](https://github.com/nisportal/IT-Azure-Automation/blob/main/Customer-User-Administration/CustomerUserAdministrationApp/NISHQ-Management/CreateCustomerUser.ps1 "CreateCustomerUser")

- Customer Change [here](https://github.com/nisportal/IT-Azure-Automation/blob/main/Customer-User-Administration/CustomerUserAdministrationApp/NISHQ-Management/CustomerChange.ps1 "CustomerChange")

- Customer Update [here](https://github.com/nisportal/IT-Azure-Automation/blob/main/Customer-User-Administration/CustomerUserAdministrationApp/NISHQ-Management/CustomerUpdate.ps1 "CustomerUpdate")

- Customer User Administration - CUST [here](https://github.com/nisportal/IT-Azure-Automation/blob/main/Customer-User-Administration/CustomerUserAdministrationApp/NISHQ-Management/CustomerUserAdministration_CUST.ps1 "CustomerUserAdministration_CUST")

- Customer User Administration - NC-AU [here](https://github.com/nisportal/IT-Azure-Automation/blob/main/Customer-User-Administration/CustomerUserAdministrationApp/NISHQ-Management/CustomerUserAdministration_NC-AU.ps1 "CustomerUserAdministration_NC-AU")

- Customer User Administration - NC-GROUP [here](https://github.com/nisportal/IT-Azure-Automation/blob/main/Customer-User-Administration/CustomerUserAdministrationApp/NISHQ-Management/CustomerUserAdministration_NC-GROUP.ps1 "CustomerUserAdministration_NC-GROUP")

- Customer User Administration - NC-ZC [here](https://github.com/nisportal/IT-Azure-Automation/blob/main/Customer-User-Administration/CustomerUserAdministrationApp/NISHQ-Management/CustomerUserAdministration_NC-ZC.ps1 "CustomerUserAdministration_NC-ZC")
    
**Automation Account:** 

[NC-AU-Management](https://portal.azure.com/#@NordicInsuranceSoftware.onmicrosoft.com/resource/subscriptions/c0ad470d-ffcd-4347-b9c1-69d14072d136/resourceGroups/nc-au/providers/Microsoft.Automation/automationAccounts/NC-AU-Management/overview)

**Runbooks:**

- Create Customer User [here](https://github.com/nisportal/IT-Azure-Automation/blob/main/Customer-User-Administration/CustomerUserAdministrationApp/NC-AU-Management/CreateCustomerUser.ps1 "CreateCustomerUser")

**Automation Account:** 

[ZC-GROUP-Management](https://portal.azure.com/#@NordicInsuranceSoftware.onmicrosoft.com/resource/subscriptions/1d81abd7-25b0-474d-ac16-cc69e1e95fe3/resourceGroups/ZC-Group/providers/Microsoft.Automation/automationAccounts/ZC-GROUP-Management/overview)

**Runbooks:**

- Create Customer User [here](https://github.com/nisportal/IT-Azure-Automation/blob/main/Customer-User-Administration/CustomerUserAdministrationApp/ZC-GROUP-Management/CreateCustomerUser.ps1 "CreateCustomerUser")

**Automation Account:** 

[NC-GROUP-Management](https://portal.azure.com/#@NordicInsuranceSoftware.onmicrosoft.com/resource/subscriptions/6a7dec38-4b84-41be-8d15-b5e12a973feb/resourceGroups/NC-Group/providers/Microsoft.Automation/automationAccounts/NC-GROUP-Management/overview)

**Runbooks:**

- Create Customer User [here](https://github.com/nisportal/IT-Azure-Automation/blob/main/Customer-User-Administration/CustomerUserAdministrationApp/NC-GROUP-Management/CreateCustomerUser.ps1 "CreateCustomerUser")


## 3. Microsoft Flows
All Flows can be found [here](https://make.powerapps.com/environments/Default-248b066d-c6fc-4b1a-afba-4138e54e2689/logicflows?utm_source=PAMarketing&utm_medium=header&utm_campaign=signin "Flows"). You can locate the flows blicking the "Shared with me" tab on the page

| Flow Name   |   Flow explanation   | Link
|----------|:-------------|----------|
|Export SharePoint List | Fetches tables from sharepoint list and exports the data as a csv to OneDrive | [Link](https://make.powerapps.com/environments/Default-248b066d-c6fc-4b1a-afba-4138e54e2689/logicflows?utm_source=office&utm_medium=app_launcher&utm_campaign=office_referrals)
|Customer Change | Takes input from the App and passes them to the Runbook "CustomerChange" in the automation account: NISHQ-Management  | [Link](https://make.powerapps.com/environments/Default-248b066d-c6fc-4b1a-afba-4138e54e2689/logicflows?utm_source=office&utm_medium=app_launcher&utm_campaign=office_referrals)
|New Customer User | When an entry is added to sharepoint list, the runbook "CreateCustomerUser" is executed with values from the list. finally an AD sync is triggered | [Link](https://make.powerapps.com/environments/Default-248b066d-c6fc-4b1a-afba-4138e54e2689/logicflows?utm_source=office&utm_medium=app_launcher&utm_campaign=office_referrals)
|Customer Update | takes input from the App and passes them to the runbook "CustomerUpdate" in the automation account: NISHQ-Management  | [Link](https://make.powerapps.com/environments/Default-248b066d-c6fc-4b1a-afba-4138e54e2689/logicflows?utm_source=office&utm_medium=app_launcher&utm_campaign=office_referrals)


## 4. SharePoint Online lists

- New Customer User [here](https://nordicinsurancesoftware.sharepoint.com/sites/UserManagement/Lists/New%20Customer%20User/AllItems.aspx "New Customer User")

- Customer User Administration - Cust [here](https://nordicinsurancesoftware.sharepoint.com/sites/UserManagement/Lists/Customer%20Portal/AllItems.aspx "Customer User Administration")

- Customer User Administration - NC-AU [here](https://nordicinsurancesoftware.sharepoint.com/sites/UserManagement/Lists/Customer%20User%20Administration%20NCAU/AllItems.aspx "Customer User Administration - NC-AU")

- Customer User Administration - NC-ZC [here](https://nordicinsurancesoftware.sharepoint.com/sites/UserManagement/Lists/Customer%20User%20Administration%20NCZC/AllItems.aspx "Customer User Administration - NC-ZC")

- Customer User Administration - NC-Group [here](https://nordicinsurancesoftware.sharepoint.com/sites/UserManagement/Lists/Customer%20User%20Administration%20NCGroup/AllItems.aspx "Customer User Administration - NC-Group")

### Information stored in the SharePoint List
* AD enabled status
* Dialog activation status
* Customer name
* User name
* Domain Name
* NIS E-Mail
* Customer E-Mail
* Dialog Domain Name
* Password Expired
* Password Last Set
* Last Logon Date
* Password Expiry Date

## 5. Power App Functions
| Function name   |   Flows in use by function   |
|----------|:-------------:|
| Reset password | CustomerChange & CustomerUpdate_new
| Disable account in AD | CustomerChange & CustomerUpdate_new
| Activate account in AD | CustomerChange & CustomerUpdate_new
| Create user in Dialog | CustomerChange & CustomerUpdate_new
| Activate user in Dialog | CustomerChange & CustomerUpdate_new
| Disable user in Dialog | CustomerChange & CustomerUpdate_new
| Create new account | CustomerChange & CustomerUpdate_new
| Export full list of all user accounts | ExportSharepointList
| Add customer e-mail to the account | CustomerChange & CustomerUpdate_new
| Sync user | update_sharepoint_list_new, CustomerUpdate_new

# Resources

