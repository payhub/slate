#Transaction Report
This API allows the merchant to search for transactions filtering by a wide range of optional parameters.

#Request Method
`PATCH`

#Endpoint (URL to Call)
`webservice URL/api/v2/search/transactionReport`

##Required Headers
* **Content-Type**: application/json
* **Accept**: application/json
* **Authorization**: Bearer {auth_token}

##Payload (JSON Format)
```
{“parameter1”:”value”,
“parameter2”:”value”,
“parameter3”:”value”}
```


#Parameters
All the parameters are optional, they can be sent in no particular order. If one of the parameters is incorrect the server will return an error.

The parameters are case sensitive.

Below is an explanation of the parameters.

Parameter | Description
--- | -----
batchIdFrom, batchIdTo | Allows the merchant to look for transactions where the **batchId** is between the batchIdFrom and batchIdTo. <br>You can also use any one of the two parameters. In that case, the result will be the transactions with the batchId greater than **batchIdFrom**, or the batchId less than **batchIdTo**.
transactionType | Allows the merchant to look for transactions where the transaction type is any one of the following:<ul><li>Void</li><li>Sale</li><li>Refund</li><li>Verify</li></ul>
responseCode | Allows the merchant to look for transactions where the response code is equal to a specific code. For example, the code “00” means a successful transaction.
amountFrom, amountTo | Allows the merchant to look for transactions where the amount of the transaction is between the **amountFrom** and **amountTo**. <br>You can use any one of the two parameters. In that case, the result will be the transactions with the amount greater than **amountFrom**, or the amount less than **amountTo**.
firstName | Allows the merchant to look for transactions where the customer’s first name contains the value of the parameter.
lastName | Allows the merchant to look for transactions where the customer’s first name contains the value of the parameter.
phoneNumber | Allows the merchant to look for transactions where the customer’s phone number contains the value of the parameter.
email | This parameter allows the merchant to look for transactions where the customer’s email address contains the value of the parameter.
trnDateFrom, trnDateTo | Allows the merchant to look for transactions where the date of the transaction is between the trnDateFrom and trnDateTo. <br>You can use any one of the two parameters. In that case, the result will be the transactions with the date later than the trnDateFrom, or the date earlier than trnDateTo.<br>Format: “yyyy-mm-dd hh:mm:ss”.
cardType | This parameter allows the merchant to look for transactions where the card type matches the value of the parameter. <br>For instance: MasterCard, Visa.
cardLast4Digits | Allows the merchant to look for transactions where the credit card’s last four digits matches the value of the parameter. <br>Format: “XXXX”.
cardToken | Allows the merchant to look for transactions where the credit card’s token matches the value of the parameter. <br>Format: “XXXXXXXXXXXXXXXX”.
authAmountFrom, authAmountTo | Allows the merchant to look for transactions where the authorized amount of the transaction is between the authAmountFrom and authAmountTo. <br>You can use any one of the two parameters. In that case the result will be the transactions with the authorization amount is greater than **authAmountFrom**, or the authorization amount less than **authAmountTo**.
swiped | Allows the merchant to look for transactions that where performed with a swiper device. <br>Valid values: true, false
source | Allows the merchant to look for transactions where the source matches the value of the parameter. <br>For instance: “3rd Party API”
note | Allows the merchant to look for transactions where the note matches the value of the parameter.
transactionStatus | Allows the merchant to look for transactions where the transaction status matches the value of the parameter. <br>For instance: “Approved”

##Result Data
The result data is an array of objects in JSON format.

####Sample Object in JSON Format
```
{
      "amount": "3.0",
      "authAmount": "3.0",
      "batchID": "1311",
      "cardLast4Digits": "XXXXXXXXXXXX4500",
      "cardToken": "9999000000002145",
      "cardType": "MasterCard",
      "customerName": "First Contact",
      "email": "first.contact@gmail.com",
      "note": "",
      "phoneNumber": "(415) 122 4567",
      "responseCode": "00",
      "responseText": "VER UNAVAILABLE",
      "source": "3rd Party API",
      "swiped": "true",
      "transactionDate": "2015-04-02 11:46:57",
      "transactionID": "180836",
      "transactionStatus": "Approved",
      "transactionType": "Sale"
  }
```
