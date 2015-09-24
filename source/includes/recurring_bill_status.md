# Recurring Bill Status

This topic provides the information about the Recurring Bill Status API. Use the Recurring Billing Status API to change the status of any recurring bill.

## Request Method

`PATCH`

## Endpoint (URL to Call)

`http://payhub.com/payhubws/api/recurring-bill-status/{id}`

## Elements

### RecurringBillStatusInput

Key | Type | Value
--- | ---- | -----
 recurring_bill_status | string | The status of the recurring bill: ['ACTIVE' or 'PAUSED' or 'COMPLETED' or 'CANCELED' or 'DELINQUENT' or 'UNKNOWN']. <br>The possible values to change to are:<ul><li>**ACTIVE**: The recurring bill will be charged on the next bill date. You can change it to ACTIVE only if it is currently PAUSED.</li><li>**PAUSED**: The recurring bill will NOT be charged while in this state. You can change it to PAUSED only if it is currently ACTIVE. Once the bill is in PAUSE state, any bill dates that pass while a bill is paused will NOT be charged. It will be charged only when it is 'unpaused' or changed it back to ACTIVE state.</li><li>**CANCELLED**: The recurring bill will be stopped. It will not be charged again. You can change to CANCELLED only if it is currently ACTIVE or PAUSED. Once the bill is cancelled, you cannot change its status.</li></ul> **Note:** You cannot use the COMPLETED status to stop a recurring bill from charging in the future. You should use CANCELLED to stop a recurring bill.

```json
{
 "recurring_bill_status": "CANCELED"
}
```

## Response Messages

Code | Meaning | Sample code
---- | ------- | -----------
200 | (not returned) | `ResourceSupport { links (array[Link], optional) } Link { templated (boolean, optional), rel (string, optional), href (string, optional) }`
204 | The recurring bill status was successfully updated. | `Void {}`
400 | Bad request due to incorrect or invalid data passed. | `RestErrorStructure { errors ( array[RestError], optional)} RestError {   status ( string, optional) = ['100' or '101' or '102' or '103' or '200' or '201' or '202' or '203' or '204' or '205' or '206' or '207' or '208' or '226' or '300' or '301' or '302' or '302' or '303' or '304' or '305' or '307' or '308' or '400' or '401' or '402' or '403' or '404' or '405' or '406' or '407' or '408' or '409' or '410' or '411' or '412' or '413' or '414' or '415' or '416' or '417' or '418' or '419' or '420' or '421' or '422' or '423' or '424' or '426' or '428' or '429' or '431' or '500' or '501' or '502' or '503' or '504' or '505' or '506' or '507' or '508' or '509' or '510' or '511']: The Http Status Code, code ( string, optional), location ( string, optional), reason ( string, optional), severity ( string, optional) }`
401 | Unauthorized. You do not have permission to perform this operation. Make sure you entered a valid API Key at the top of the page. | `Void {}`
403 | Forbidden. The server does not permit you to use this URI. | `Void {}`
500 | Internal server error due to encoding the data, or to a PayHub server failure. Contact PayHub. | `Void {}`
