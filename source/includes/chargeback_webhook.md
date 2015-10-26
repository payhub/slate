# Chargeback Webhook

This topic provides the information about the web service that you need to create to receive requests from Chargebacks.

## Request Method:
###POST

##End Point URL
Speciefied by the Sales Office in My Office Admin - MyOffice
##Request Elements (from Safyer)

<aside class='warning'>All fields are required unless indicated otherwise</aside>

|Parameter | Type | Description|
|--------- | --------- | ------------ |
|internalID|String 5 - 20 digits|The unique system identifier for the merchant issued by Safyer.|
|mid|String 5 - 20 digits|The unique Merchant Identification Number (MID) issued by the processor.|
|amount|String <br> money|Chargeback amount.|
|AuthorizationCode|String 1 - 6 characters|Chargeback authorization code.|
|CardNumLast4|String 4 digits|Card number, last four.|
|CardType|String 4 - 16 characters|Card name.<br> Possible values:<ul><li>Visa</li><li>MasterCard</li><li>American Express</li><li>Not defined</li>|
|ChReasonCode|String 1 - 5 characters ||
|ReceivedDate|Date <br> Date format:(MM-dd-yyyy)|Date of Chargeback received.|
|TransactionDate|Date<br>Date format:(MM-dd-yyyy)|Date of Chargebacks transaction|

##Response Elements (to Safyer)

|Parameter | Type | Description|
|--------- | --------- | ------------ |
|code|String<br>1 digit|The response code returned in the response.<br><ul><li> 0: Success</li><li>1: Error</li></ul>|
|msg|String 2 - 5 characters|If code is 0 then msg is 'OK'. Otherwise, msg is 'ERROR'|