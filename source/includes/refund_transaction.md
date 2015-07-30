#Refund Transaction
This topic provides the information the Refund transaction.

Run the Refund transaction when you want to refund the full amount of a settled sale, which is not yet refunded. The refund amount will be the same as the total amount that was originally charged in the sale. The amount is refunded to the credit or debit card that was originally charged.

#Request Method


#Endpoint (URL to Call)
`http://payhub.com/payhubws/api/refund`

#Elements
(All Required)

Element | Type | Value
--- | ---- | -----
 organization_id | integer | The organization id of the merchant who did the sale that you are trying to refund.
 terminal_id | integer | The terminal id of the merchant from where the sale is done.
 record_format | string | To specify if the sale has to be made over a credit card, debit card or cash payment. <br>The accepted values are: <ul><li>CASH_PAYMENT</li><li>CREDIT_CARD</li><li>DEBIT_CARD</li></ul>
 transaction_id | integer | The transaction id of the sale you want to refund.

##Example of JSON
```
{
"merchant" : {
 "organization_id" : 10005,
 "terminal_id" : 5

 },

 "record_format":"CREDIT_CARD",
 "transaction_id":"114"
}
```
