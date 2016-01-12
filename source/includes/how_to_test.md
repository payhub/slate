# How to Test

To run test transactions, simply set the "mode" parameter to "demo" and all transactions will automatically be routed through our test system. When in demo mode, the "orgid", "username", "password", and "tid" fields are optional. 

##Use the following test data to run test transactions:

###Test Data

|Card Type|Card #|Card CVV|Transaction Amount|Expected Result|
|-----|-----|-----|-----|-----|
|Visa|4012881888818888|999|10.00|Success|
|Visa|4266841082854082|999|0.01|Fail|
|MasterCard|5466410004374507|998|10.00|Success|
|MasterCard|5454545454545454|998|0.20|Fail|
|American Express|371449635398431|9997|10.00|Success|
|American Express|371449635398431|9997|0.20|Fail|
|Discover|6011000990156527|996|10.00|Success|

<aside class="notice">
The expiration date used can be any time in the future. 
To test AVS, just provide any valid address or zip information with the request and you will get a successful AVS response. To test for a failed CVV code, use a different CVV than what is specified in the table above.
</aside>