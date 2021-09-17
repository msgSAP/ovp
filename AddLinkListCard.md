# Add a link list card to the Overview Page

### 1. Create a CDS View Entity ZRAPH_##_C_OVPTraveler
Base this new view entity on /dmo/customer.  
  
| Source                                      | Field name   | Is key |
| ------------------------------------------- | ------------ | ------ |
| */dmo/customer.*customer_id                 | TravelerID   | Yes    |
| */dmo/customer.*first_name                  | FirstName    | No     |
| */dmo/customer.*last_name                   | LastName     | No     |
| concat_with_space(first_name, last_name, 1) | FullName     | No     |
| */dmo/customer.*email_address               | EMailAddress | No     |
| */dmo/customer.*phone_number                | PhoneNumber  | No     |
  
Add the following annotations to provide the metadata for the view entity:  
  
__To the view entity itself__
```abap
@UI.headerInfo: {
  typeName: 'Traveler',
  typeNamePlural: 'Travelers',
  title: {
    label: 'First Name',
    value: 'FirstName',
    type: #STANDARD
  },
  description: {
    label: 'Name',
    value: 'FullName',
    type: #STANDARD
  }
}
```
  
__FullName:__  
```abap
@Semantics.name.fullName: true
```
  
__EMailAddress:__  
```abap
@Semantics.eMail.address: true
@Semantics.eMail.type:  [#WORK, #PREF]
```
  
__PhoneNumber:__  
```abap
@Semantics.telephone.type:  [ #PREF , #WORK]
```
  
Activate ZRAPH_##_C_OVPTraveler.  
  
[__Solution__](./solutions/AddLinkListCard/ZRAPH_%23%23_C_OVPTraveler.txt)  
  
### 2. Expose ZRAPH_##_C_OVPTraveler as entity set
Adapt ZRAPH_##_SD_OVP:  

| CDS View Entity        | Entity Set |
| ---------------------- | ---------- |
| ZRAPH_##_C_OVPTraveler | Traveler   |
  
Activate ZRAPH_##_SD_OVP.  
  
[__Solution__](./solutions/AddLinkListCard/ZRAPH_%23%23_SD_OVP.txt)  
  

[<< Previous Step](./AddStackCard.md) | [Next Step >>](./AddAnalyticalCard.md)
