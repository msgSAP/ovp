# Add a list card to the Overview Page

### 1. Create CDS View Entity ZRAPH_##_C_OVPMostBookedHotels
Base this new view entity on ZRAPH_##_I_RoomReservationWDTP.  
  
| Source                          | Field name        | Is key |
| ------------------------------- | ----------------- | ------ |
| *ZRAPH_##_I_TravelWDTP.*HotelID | HotelID           | Yes    |
| _Hotel.name                     | HotelName         | No     |
| count( * )                      | ReservationsCount | No     |
  
Add the following annotations to provide the metadata for the table:  
  
__HotelName:__  
```abap
@UI.lineItem: [{
    qualifier: 'List',
    label: 'Hotel',
    position : 1
}]
```
  
__ReservationsCount:__  
```abap
@UI.lineItem: [{
    qualifier: 'List',
    label: 'Reservation Count',
    position : 2
}]
```
  
Activate ZRAPH_##_C_OVPMostBookedHotels.  
  
[__Solution__](./solutions/AddListCard/ZRAPH_%23%23_C_OVPMostBookedHotels-1.txt)  
  
### 2. Expose ZRAPH_##_C_OVPMostBookedHotels as entity set
Adapt ZRAPH_##_SD_OVP:  

| CDS View Entity                | Entity Set       |
| ------------------------------ | ---------------- |
| ZRAPH_##_C_OVPMostBookedHotels | MostBookedHotels |
  
Activate ZRAPH_##_SD_OVP.  
  
[__Solution__](./solutions/AddListCard/ZRAPH_%23%23_C_OVPMostBookedHotels-1.txt)  
  
  
[<< Previous Step](./AddTableCard.md) | [Next Step >>](./AddStackCard.md)
