# Create initial Overview Page

### 1. Create the Interface CDS View Entity ZRAPH_##_I_OVPFilter
This CDS View Entity (together with its consumption view) represents the filter options for the Overviewpage.  
We want to filter for the overall status and customers home country.  
  
As primary source for the CDS View Entity we use ZRAPH_##_I_TravelWDTP.  
To get access to the customers home country, we join in /DMO/I_Customer.  
  
Following fields need to be put in the projection list:  

| Source                              | Field name          | Is key |
| ----------------------------------- | ------------------- | ------ |
| ZRAPH_##_I_TravelWDTP.TravelID      | TravelID            | Yes    |
| ZRAPH_##_I_TravelWDTP.OverallStatus | OverallStatus       | No     |
| /DMO/I_Customer.CountryCode         | CustomerHomeCountry | No     |
  
[__Solution__](./solutions/CreateInitialOVP/ZRAPH_%23%23_I_OVPFilter-1.txt)

### 2. Create the Consumption CDS View Entity ZRAPH_##_C_OVPFilter
This CDS View Entity adds annotations for the filtering of the OVP.  
It shall be created as a selection from ZRAPH_##_I_OVPFilter and projects all of its fields.  
  
[__Solution__](./solutions/CreateInitialOVP/ZRAPH_%23%23_C_OVPFilter-1.txt)

### 3. Create the service definition ZRAPH_##_SD_OVP
Expose the following entity:  

| CDS View Entity      | Entity Set |
| -------------------- | ---------- |
| ZRAPH_##_C_OVPFilter | OVPFilter  |
  
[__Solution__](./solutions/CreateInitialOVP/ZRAPH_%23%23_SD_OVP-1.txt)

### 4. Create the service binding ZRAPH_##_SB_OVP and publish it
Use service definition ZRAPH_##_SD_OVP.  
Use Binding Type: OData V2 - UI.  

### 5. Create Overview Page in Business Application Studio and test it

#### Open the Business Application Studio (BAS).  
Go to [https://hana.ondemand.com](https://hana.ondemand.com).
Sign in to your trial account.  
Enter your trial account using the button "Go To Your Trial Account".  
Enter your trial subaccount.  
On the left pane, click on "Instances and Subscriptions".  
There you should already have one subscription to the SAP Business Application Studio.  
Click on that.  
Choose your devspace (which you should already have created).  

#### Create new project
Click on "Start from template" in the "Welcome"-Tab.  
![(BAS Create Project)](./images/CreateInitialOVP/BAS-Create-Project-1.png)  
Or press CTRL+SHIFT+P and type "New Project" and select the item "SAP Business Application Studio: New Project from Template".  
![(BAS Create Project)](./images/CreateInitialOVP/BAS-Create-Project-2.png)  
Choose "SAP Fiori Application" and press the "Start" button below.  
  
As Application type, take "SAP Fiori Elements".  
Press on "Overview Page" and press the "Next" button below.  
  
As data source choose "Connect to a System".  
As system choose your destination, created in previous lessons.  
As service choose the service ZRAPH_##_SB_OVP, that you published earlier.  
Press the "Next" button below.  
  
As filter entity choose the entity OVPFilterType (The "Type" part was added automatically by the system for the OData Service) we defined in the service definition.  
Press the "Next" button below.  
  
As module name choose "travel_ovp_##".  
As application title choose "Travel Overview".  
As namespace choose "com.erp.ovp".  
As description choose "Travel Overview".  
Leave the project folder path at "/home/user/projects".  
Also leave all 3 radio buttons at "No".  
Press the "Finish" button below.  
  
At the right in the project explorer you see a new folder "travel_ovp_##".  
![(BAS Create Project)](./images/CreateInitialOVP/BAS-Create-Project-3.png)  

#### Test the application
Go to the run configuration pane and press the run button on the first of the 3 automatically created configurations.  
![(BAS Test App)](./images/CreateInitialOVP/BAS-Run-1.png)  
Or press CTRL+SHIFT+P and type "Preview App" and select the item "Fiori: Preview Application".  
![(BAS Test App)](./images/CreateInitialOVP/BAS-Run-2.png)  
  
Your app should now look like this:  
![(BAS Test App)](./images/CreateInitialOVP/BAS-Run-3.png)  
  
If you press on "Adapt Filters", the 3 fields we defined in the Consumption View Entity should be visible.  
![(BAS Test App)](./images/CreateInitialOVP/BAS-Run-4.png)  

### 6. Add filter fields to filter bar and rename them
This is not how we want our filter bar to look.  
We want to have the filter fields available directly on the filter bar without needing to adapt them.  
Also we want different labels not show the travel id at all.  
Therefore we add the following annotations to ZRAPH_##_C_OVPFilter:  
  
__TravelID:__
```abap
@UI.hidden: true
```
__OverallStatus:__
```abap
@UI.selectionField: [{ position: 1 }]
@EndUserText.label: 'Status'
```
__CustomerHomeCountry:__
```abap
@UI.selectionField: [{ position: 2 }]
@EndUserText.label: 'Customer Home Country'
```
  
After activating ZRAPH_##_C_OVPFilter again, we test our app once more (if you have the app still open, reloading it suffices) and it should look like this:  
![(BAS Test App)](./images/CreateInitialOVP/BAS-Run-5.png)  
![(BAS Test App)](./images/CreateInitialOVP/BAS-Run-6.png)  
  
However when pressing the value help button, only a generic popup shows up.  
![(BAS Test App)](./images/CreateInitialOVP/BAS-Run-7.png)  
  
[__Solution__](./solutions/CreateInitialOVP/ZRAPH_%23%23_C_OVPFilter-2.txt)

### 7. Add useful value helps for the filter fields

#### Add status entity to service definition ZRAPH_##_SD_OVP
Expose the following entity:  

| CDS View Entity       | Entity Set |
| --------------------- | ---------- |
| ZRAPH_I_OverallStatus | Status     |
  
[__Solution__](./solutions/CreateInitialOVP/ZRAPH_%23%23_SD_OVP-2.txt)

#### Add association to the status to ZRAPH_##_I_OVPFilter
The association target is ZRAPH_I_OverallStatus.  
The connection is done using fields TravelStatuns and overall_status of the corresponding views.  
Alias the association with _Status and expose it in the projection list.  
Activate ZRAPH_##_I_OVPFilter.  

[__Solution__](./solutions/CreateInitialOVP/ZRAPH_%23%23_I_OVPFilter-2.txt)

#### Expose the new association in the consumption view ZRAPH_##_C_OVPFilter
Add the association to the projection list.  
Add the following annotation to field OverallStatus of ZRAPH_##_C_OVPFilter:  
  
```abap
@Consumption.valueHelpDefinition: [{entity: { name:    'ZRAPH_I_OverallStatus',
                                              element: 'TravelStatus' `} }]
```
Activate ZRAPH_##_C_OVPFilter.  
  
Check the service binding ZRAPH_##_SB_OVP.  
It should now include the following new lines:  
![(Service Binding)](./images/CreateInitialOVP/Eclipse-Service-Binding-2.png)  
  
Again check the app. Press the value help for the status field. It should now look like this:  
![(BAS Test App)](./images/CreateInitialOVP/BAS-Run-8.png)  
  
[__Solution__](./solutions/CreateInitialOVP/ZRAPH_%23%23_C_OVPFilter-3.txt)
  
  
[Next Step >>](./AddTableCard.md)