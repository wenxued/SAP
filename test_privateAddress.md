

`https://workforcebusinessservicesh-trainingwbs-wenxue-workforce4672e1e9.cfapps.sap.hana.ondemand.com/odata/v4/WorkforceAPI/WorkforcePersons?$expand=workAssignments($expand= paymentDetails,privateAddresses)&filterPrivateAddress=false`





S/4 **sets** parameter: filterPrivateAddress = true

 

MDW API checks that: 

- If  payment is alone requested without address it’s ok. 

`$expand=workAssignments($expand=paymentDetails) &filterPrivateAddress = true`

- If only address is requested **without expanding payment** then its request **error** and will be **rejected**. Someone is trying to do anti-DPP config

  `$expand=workAssignments($expand=privateAddresses),privateAddresses&filterPrivateAddress = true`

  

- If  both address and payment are expanded then service checks if there is  corresponding bank and cheque payment and removes address from response payload

`$expand=workAssignments($expand=privateAddresses,paymentDetails),privateAddresses&`

 

 

If S/4 **does not set** the parameter filterPrivateAddress  or s**ets it to false** then server sends actual entities requested without any specific DPP logic.





1. 普通情况
2. **deltaRequest**



workforcebusinessservicesh-trainingwbs-candy-workforce-27ee6758.cfapps.sap.hana.ondemand.com 