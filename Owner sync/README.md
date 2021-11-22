# Dual Write - Sync Customer Employee responsible with Account Owner field

## Problem
Microsoft Dual Write solution doesn’t support the Dataverse Owner field sync with FSC Employee responsible (Worker) field.


## Solution
It is possible to update the Dataverse Owner field by the Power Automate flow. 
![image](https://user-images.githubusercontent.com/22589392/142865508-1ca83e1f-7bd8-4675-aaba-9e243d02bbca.png)
Power Automate flow is going to be triggered on each Account record creation or record update. Depending on the *Update from Dual Write* value, it decides from which side it is necessary to copy the Owner value. If the *Update from Dual Write* value is equal to true, then the FSC side (or Dual Write) initiates the data sync. Therefore, the Worker field value should be copied to the Owner field. Else, Dataverse is the initiator, and the Owner field value should be copied to the Worker field.
In order to match Worker with User, an email address will be used.

### Limitations and assumptions
Only user records could be used as a value for the owner field.
Default general Dataverse account should be used when the Worker field in FSC is empty.
Workers should have defined their primary email address. This value should be the same as in the Dataverse in order to match Worker and User.
Unfortunately, several record updates will be triggered by this Power Automate flow.
This is an example of the Owner field sync for the Account record. Other entities with the Owner field could be updated the same way.

## Solution Installation
### Run Workers (cdm_worker) Dual Write sync
### Create two new custom fields on Accounts
![image](https://user-images.githubusercontent.com/22589392/142865859-0d09a118-8f86-4051-bc15-6253ac5f74b7.png)

![image](https://user-images.githubusercontent.com/22589392/142866146-46d5e9e3-2625-4491-ab58-bcef83ca5356.png)
![image](https://user-images.githubusercontent.com/22589392/142866246-7c6e572b-dd96-4818-a4d4-24d93085a993.png)

### Add custom fields to the Dual Write mappings
Open Dual Write, locate *Customers V3 (accounts)*. Make sure that it is stopped, then add new custom fields to the mapping of the fields.
![image](https://user-images.githubusercontent.com/22589392/142866492-17d7f621-39fa-4722-8e72-0503f2b031e0.png)

Create the following transformation for the Updated from Dual Write field
![image](https://user-images.githubusercontent.com/22589392/142866537-4845d513-af27-44d6-8543-feb0c4705935.png)

After that, you can, optionally, start the sync. Make sure it is successful.

### Power Automate flow

Import Power Automate flow. Adjust it. 
Make sure that you have a general Dataverse user account email updated.
![image](https://user-images.githubusercontent.com/22589392/142867930-65210fa0-44d2-4ec7-9272-a69dfc29d05a.png)

Open Dual Write, locate *Customers V3 (accounts)*. Stop it if it's running. Then, start with the initial sync.
Make sure that you have no failed Power Automate flow runs.
Make some functional tests.

