# Bot Triage, Handoff and Escalation to a human agent - sample scenario 
This repository contains a healthbot service(.hbs) file which consists of some sample Assets (such as Scenarios and Language models) required to quickly demonstrate the integration of Microsoft Health Bot service with Microsoft Cloud for Healthcare solutions.

It provides an example of a multi-turn welcome scenario, a patient triage scenario and the ability to escalate a conversation to a human agent in Patient Service Center at any point during the conversation. 

This should enable you to quickly demo a full-fledged healthbot and its escalation capabilities from Patient Access Portal's chat widget.

## Prerequisites
Before deploying the samples scenario make sure that: 
*	[Microsoft Cloud for Healthcare solutions](https://docs.microsoft.com/en-us/dynamics365/industry/healthcare/deploy) have been successfully deployed. 
*	[Microsoft Healthcare bot instance](https://docs.microsoft.com/en-us/healthbot/quickstart-createyourhealthcarebot) is created 

__NOTE: The Microsoft Health Bot service must be configured in the same Azure Active Directory (Azure AD) tenant as your Dynamics 365 or Microsoft Dataverse environment used for Patient Service Center.__
*	No scenarios are created in this instance as during the samples deployment if the assets in this repository file have names that conflict with the assets in your bot, they will overwrite the existing bot assets.

## Deployment

1.	Download the repository into your local folder.
2.	On the top right corner, click on Settings and then click on Restore button.

![](./BAS-Ecosystem-Engineering/Microsoft-Cloud-For-Healthcare/Healthbot-Service-Samples/Triage-Handoff-Escalation/Images/SettingsScreen.png)
 
3.	Use the file picker to locate the downloaded hbs file, type Restore in the bottom field and click on Restore button.

![](./RestoreScreen.png)
 
4.	Navigate to Intergation >> Channels and enable the "Teams" channel (if not already enabled). Click the "View" option and copy your bot ID to the clipboard.

![](https://github.com/microsoft/BAS-Ecosystem-Engineering/blob/main/Microsoft-Cloud-For-Healthcare/Healthbot-Service-Samples/Triage-Handoff-Escalation/Images/EnableTeams.png)
 
__NOTE: The below steps are required only if you haven’t__

* Created a bot user in Dynamics 365
* Configured Queues, workstreams and chat widgets in Dynamics 365
* Embedded chat widget code in Portal Management.
5.	Create a bot user by following the below steps:

    a.	Navigate to your Dynamics 365 organization and go to Settings > Security > Users.
    
    b.	In the view drop-down, select Application Users.
    
    c.	Select New.
    
    d.	In the Uses drop-down, select Application User.
    
    e.	On the New User page, enter or select the following information:
    

*   User Name: User name of the bot. It is not displayed in the chat widget.

*  Application ID: An application ID for any valid (non-expired) application created in Azure Active Directory (Azure AD) for the same tenant. It is not used by the bot in Omnichannel for Customer Service.

* Full Name: Name of the bot to be displayed in the chat widget.

*  Primary Email: Enter a dummy email address. It is not used by the bot in Omnichannel for Customer Service.

*   User type: Select Bot application user.

*  Bot application ID: Bot's ID you copied in the previous step(Step 4).
For more information on creating an application user, see [Create an application user](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/use-multi-tenant-server-server-authentication#create-an-application-user--associated-with-the-registered-application--in-).

    f. Save the record.
    
    g. Select Manage Roles on the command bar.
    
    h.	In the Manage User Roles window, select Omnichannel agent, and then select OK.
6.	Configure a default queue with basic routing in the underlying Dynamics 365 Omnichannel application.

    a.	Navigate to the Dynamics 365 Omnichannel Administration App.
    
    b.	Open the Queues list.
    
    c.	Open Default Queue and make sure the priority is set high enough and that, under Users (Agents), only the bot user that you created earlier is listed. Ensure that the bot capacity in the user record is set appropriately. [Learn more about capacity](https://docs.microsoft.com/en-us/dynamics365/omnichannel/administrator/users-user-profiles#capacity)
    
    d.	Create a new queue to escalate to your service agents with a priority lower than your default queue with the bot. Add the appropriate service agents to the Users (Agents) list. Don't add the bot application user here.
    
    e.	Open the work stream associated with the chat widget, or create and select a new one, while setting the channel to Live chat. [Learn more about work streams](https://docs.microsoft.com/en-us/dynamics365/omnichannel/administrator/work-streams-introduction)
    
    f.	With the work stream record opened, open the Context variables tab and create a new context variable. Ensure that Type is set to Number.
    
    g.	Navigate back to the work stream and open the Routing rule tab.
    
        •	Create a new rule for the default queue with the Health Bot without a condition.
        
        •	Create a new rule for the Agent queue with a condition (for example: Context variable > EscalateToAgent > Equals > 1).
        
    h.	Create a new chat widget and associate the work stream with the chat widget.

7.	Embed Microsoft Health bot into Patient Access Portal.

    a.	In Patient Service Center, navigate to the chat widget record you want to embed and open it.

    b.	Copy the code snippet under Widget snippet. This code will be used to embed the chat into the portal.

    c.	Navigate to your Portal Management App and open the Content Snippets list.

    d.	Look for an existing Chat Widget Code snippet and open it.

    e.	In the Value > HTML window, paste your widget snippet code from Step 7(b) and save.

    f.	Restart the Patient Access Portal from Power Platform Admin Center.

Now you can navigate to the Patient Access Portal and follow the steps in the portal’s chat widget to demonstrate the sample healthbot scenarios which was imported from this repo.
