***
*Name:* NIS User Administration IT - Power App Design

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
    - [A. The Power App](#a-the-power-app)
    - [B. Azure Automation](#b-azure-automation)
    - [C. Microsoft Flows](#c-microsoft-flows)
    - [D. Sharepoint Online Lists](#d-sharepoint-online-lists)
5. [Resources](#Resources)

# Purpose
The purpose of this document is to provide walkthrough of how the Power App: [NIS User Administration IT](https://apps.powerapps.com/play/e/default-248b066d-c6fc-4b1a-afba-4138e54e2689/a/a1be06a5-87d1-4191-8e41-3f49f1ba0ec5?tenantId=248b066d-c6fc-4b1a-afba-4138e54e2689) is designed. 

# Scope
The scope of this document is limited to the technical design of the "NIS User Administration IT" app, along with it's dependencies like, Azure automation accounts, Microsoft Flow, Sharepoint Online, and PowerShell. This document will not cover the use of the app, nor the process for managing users with it.

# Definitions
Power App - Refers to Microsoft Power Apps
Flow - Microsoft Flow

# Procedures
We will first list and reference all resources used to power the functions of the Power App. Then we will cover the functionality and how it's all connected. 

## A. The Power App 
[NIS User Administration IT](https://apps.powerapps.com/play/e/default-248b066d-c6fc-4b1a-afba-4138e54e2689/a/5df33000-1ee4-4b29-a6f2-c71df8591b70?tenantId=248b066d-c6fc-4b1a-afba-4138e54e2689&source=portal&skipmetadata=true)

## B. Azure Automation
**Automation Account:** 

[NISHQ-Management](https://portal.azure.com/#@NordicInsuranceSoftware.onmicrosoft.com/resource/subscriptions/ecd44859-06e3-4231-9db8-c6d26070358f/resourceGroups/NISHQ-Management/providers/Microsoft.Automation/automationAccounts/NISHQ-Management/overview)

**Runbooks:**

- CreateCase-ToCreateUSer [here](https://github.com/nisportal/IT-Azure-Automation/blob/main/Customer-User-Administration/CustomerUserAdministrationApp/NISHQ-Management/CreateCustomerUser.ps1 "CreateCase-ToCreateUSer")

## C. Microsoft Flows
All Flows can be found [here](https://make.powerapps.com/environments/Default-248b066d-c6fc-4b1a-afba-4138e54e2689/logicflows?utm_source=PAMarketing&utm_medium=header&utm_campaign=signin "Flows"). You can locate the flows blicking the "Shared with me" tab on the page

| Flow Name   |   Flow explanation   | Link
|----------|:-------------|----------|
|New NIS EMP - 2 - Send mail when marked as created, and create user | Fetches tables from sharepoint list "New employee information" and starts script "CreateNewNISEmployee" in automation account: NISHQ-Management. sends email to IT that the user is created | [Link](https://make.powerapps.com/environments/Default-248b066d-c6fc-4b1a-afba-4138e54e2689/logicflows?utm_source=office&utm_medium=app_launcher&utm_campaign=office_referrals)
|New NIS EMP - 3 - Send mail when marked as done, mail to IT and creater  | Fetches tables from sharepoint list "New employee information" and sends an email to IT. Removes the user entru from the IT list | [Link](https://make.powerapps.com/environments/Default-248b066d-c6fc-4b1a-afba-4138e54e2689/logicflows?utm_source=office&utm_medium=app_launcher&utm_campaign=office_referrals)


## D. SharePoint Online lists

- New Employee Information [here](https://nordicinsurancesoftware.sharepoint.com/sites/UserManagement/Lists/New%20Employee%20Information/AllItems.aspx)
- Disable NIS Employee [here](https://nordicinsurancesoftware.sharepoint.com/sites/UserManagement/Lists/Disable%20NIS%20Employee/AllItems.aspx)

### Information stored in the "New Employee Information" SharePoint List
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


## e. How it's connected
**onboarding**
When a user is 'onboarded' by IT in the App, it is because HR has pre-created the account. IT will be notified by the Dialog case, and email sent to IT by the HR Power App.
IT markes the user as created with the toggle "*Employee Created" in the app. This triggers the flow: "New NIS EMP - 2 - Send mail when marked as created, and create user*". This runs a runbook: "*CreateNewNISEmployee*" in an azure automation account: "*NISHQ-Management*". It passes the values from the sharepoint list "New Employee Information". firstName, lastName, department, jobTitle, countryValue, createdByEmail, privateEmail, phoneNumber. This script will create the account, it will then send an email to the IT department.

The user will stay in the list in the IT app until the user is marked as done, this is an extra reminder to order IT equipment etc. when the hardware is installed and the user is tested we mark the user as done with the toggle "Employee Done" in the PowerApp


**offboarding**
When a new entry is created in the sharepoint list "Disable NIS Employee" by the HR app, the flow "*Disable user - 2 - marked as disabled - disable user and send mail to IT*" is triggered. This flow will execute the runbook "Disable-Employee-Account" which will disable the user account in multipe domains

# Resources

