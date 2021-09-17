# Add a list card to the Overview Page

### 1. Create CDS View Entity ZRAPH_##_C_OVPMostBookedHotels
Base this new view entity on ZRAPH_##_I_RoomReservationWDTP.  
  
| Source                          | Field name        | Is key |
| ------------------------------- | ----------------- | ------ |
| *ZRAPH_##_I_TravelWDTP.*HotelID | HotelID           | Yes    |
| _Hotel.name                     | HotelName         | No     |
| count( * )                      | ReservationsCount | No     |
  
[__Solution__](./solutions/AddListCard/ZRAPH_%23%23_C_OVPMostBookedHotels-1.txt)  
  
  
[<< Previous Step](./AddTableCard.md) | [Next Step >>](./AddStackCard.md)
