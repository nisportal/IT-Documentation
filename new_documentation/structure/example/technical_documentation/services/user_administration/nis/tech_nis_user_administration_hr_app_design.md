***
*Name:* NIS User Administration HR - Power App Design

*Version:* 1.0.0

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
The purpose of this document is to provide walkthrough of how the Power App: [NIS User Administration HR](https://apps.powerapps.com/play/e/default-248b066d-c6fc-4b1a-afba-4138e54e2689/a/5df33000-1ee4-4b29-a6f2-c71df8591b70?tenantId=248b066d-c6fc-4b1a-afba-4138e54e2689&source=portal&skipmetadata=true) is designed. 

# Scope
The scope of this document is limited to the technical design of the "NIS User Administration HR" app, along with it's dependencies like, Azure automation accounts, Microsoft Flow, Sharepoint Online, and PowerShell. This document will not cover the use of the app, nor the process for managing users with it.

# Definitions
Power App - Refers to Microsoft Power Apps

# Procedures
We will first list and reference all resources used to power the functions of the Power App. Then we will cover the functionality and how it's all connected. 

## 1. The Power App 
[NIS User Administration HR](https://apps.powerapps.com/play/e/default-248b066d-c6fc-4b1a-afba-4138e54e2689/a/5df33000-1ee4-4b29-a6f2-c71df8591b70?tenantId=248b066d-c6fc-4b1a-afba-4138e54e2689&source=portal&skipmetadata=true)

## 2. Azure Automation
**Automation Account:** 

[NISHQ-Management](https://portal.azure.com/#@NordicInsuranceSoftware.onmicrosoft.com/resource/subscriptions/ecd44859-06e3-4231-9db8-c6d26070358f/resourceGroups/NISHQ-Management/providers/Microsoft.Automation/automationAccounts/NISHQ-Management/overview)

**Runbooks:**

- CreateCase-ToCreateUSer [here](https://github.com/nisportal/IT-Azure-Automation/blob/main/Customer-User-Administration/CustomerUserAdministrationApp/NISHQ-Management/CreateCustomerUser.ps1 "CreateCase-ToCreateUSer")

## 3. Microsoft Flows
All Flows can be found [here](https://make.powerapps.com/environments/Default-248b066d-c6fc-4b1a-afba-4138e54e2689/logicflows?utm_source=PAMarketing&utm_medium=header&utm_campaign=signin "Flows"). You can locate the flows blicking the "Shared with me" tab on the page

| Flow Name   |   Flow explanation   | Link
|----------|:-------------|----------|
|New NIS EMP - 1 - When an new user in list - Send an email to creator and IT | Fetches tables from sharepoint list and runs automation script to create a dialog case and send 2 emails | [Link](https://make.powerapps.com/environments/Default-248b066d-c6fc-4b1a-afba-4138e54e2689/logicflows?utm_source=office&utm_medium=app_launcher&utm_campaign=office_referrals)
|Disable user - 1 - when new user in list | Fetches tables from sharepoint list and runs automation script to create a dialog case and send 2 emails | [Link](https://make.powerapps.com/environments/Default-248b066d-c6fc-4b1a-afba-4138e54e2689/logicflows?utm_source=office&utm_medium=app_launcher&utm_campaign=office_referrals)

## 4. SharePoint Online lists

- New Employee Information [here](https://nordicinsurancesoftware.sharepoint.com/sites/UserManagement/Lists/New%20Employee%20Information/AllItems.aspx)
- Disable NIS Employee [here](https://nordicinsurancesoftware.sharepoint.com/sites/UserManagement/Lists/Disable%20NIS%20Employee/AllItems.aspx)

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

### Information stored in the "Disable NIS Employee" SharePoint List
* Initials
* Last day
* Created
* Created by
* User disabled

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

## 6. How it's connected
**onboarding**
When a user is 'onboarded' by HR in the App, it passes the information from the Power App and creates an entry in the Sharepoint list "New Employee Information". This triggers the Microsoft flow It then creates a Dialog case with the parameters from the sharepoint list: firstName, lastName, startDate, notes, createdByEmail.

it sends an email to the person who created the uer with some basic information. It also sends an email to itsupport@nisportal.com; informing IT about a pending user creation in their app "NIS User Administration IT", and that a dialog case has been created.

**offboarding**
When a user is 'offboarded' by HR in the App, it passes the information from the Power App and creates an entry in the Sharepoint list "Disable NIS Employee". This triggers the Microsoft flow "Disable user - 1 - when new user in list" which then starts an azure automation runbook that creates a Dialog case with the parameters from the sharepoint list: initials, lastDay.

# Resources

