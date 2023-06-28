# generate-an-incremented-hubspot-object-number-unique-id-based-on-previous-object

## What is it?
This coded files are intented to be used in a hubspot contact workflow within custom coded actions. Object Ids in HubSpot are randomly generated. These custom coded actions lets you generate an incremented (serial or sequential) number id (i.e. first object created get id 1, second gets 2, etc.). The example used in th code in this repo is for a custom object named "Project" but this could be uses on any object (Contacts, companies, deals, tickets, custom objects)

## How it works:

1. Create a private app within your HubSpot portal and name it something like SERIAL_RECORD_ID_GENERATOR (could be something else)
2. Give proper scopes to the private app. This depends the object on which you wish to generate a serial number id . You need read access to the object so this could be one of "crm.objects.contacts.read", "crm.objects.companies.read", "crm.objects.deals.read", "crm.objects.custom.read" or "tickets"
3. Create a number property on the object you wish to generate a serial number id. In this example the property is named "serial_number_id" but it can anything else. Don't forget to make the property "unique" in the rules tab before saving your property.
4. Create a blank workflow for the object you wish to generate a serial number id 
5. Set trigger to "create date is known" so all object records would be enrolled in the workflow. If you don't want a serial number id for all the records you can select whatever criteria you want.
6. Add a custom coded action to the workflow
7. Create a secret with your private app token and name it SERIAL_RECORD_ID_GENERATOR (could be something else)
8. Select this secret in your custom coded action
9. Select the properties "Record ID" and "Create date" as  inputs in your custom coded action
10. Copy/paste the content of the init.js file in the code editor of your custom coded action
11. Select outputs: {latestProjectId: number, projectNumber: number, nextAction: enumeration {options:"setProjectNumber", "retry"}}
12. Save your custom coded action
13. Add a value equals branch action next to your custom coded action in the workflow. Branch on the nextAction options of your first custom coded action.
14. In the "setProjectNumber" branch, copy projectNumber to serial_number_id
## OPTIONAL - If for some reason you have a long processing time and some of your record doesn't get a serial number generated you can add the following
13. In the "retry" branch, add a delay action of 5 minutes (could be less or more as needed)
14. Next to the delay action, add a 2nd custom coded action to the workflow
15. Select SERIAL_RECORD_ID_GENERATOR secret in your custom coded action
16. Select latestProjectId as an input in your custom coded action
17. Copy/paste the content of the retry.js file in the code editor of your custom coded action
17. Select outputs: {projectNumber: number, nextAction: enumeration {options:"setProjectNumber", "retry"}}
18. Save your 2nd custom coded action
19. You can repeat the retry process as much as needed
24. Set your workflow to "on"


## How the workflow should look:
![HubSpot Workflow to generate a serial number id](/hubspot-worklfows-custom-coded-actions-to-generate-a-serial-number-id.png)

