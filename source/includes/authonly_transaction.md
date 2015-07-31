#AuthOnly Transaction
This topic provides the information about the **AuthOnly** transaction.

You can use the AuthOnly transaction to authorize an amount on your customer's credit card without making the actual settlement of that amount. With the AuthOnly transaction, you actually reserve an amount for a certain period against the credit limit of the card holder.

Usually, you use the AutOnly transaction type when you are selling an item, which is currently out of stock. So, you run the AuthOnly transaction when the item is not available and when the item becomes available in the stock, you run the Capture transaction to actually make the settlement for the reserved amount. To simply say, the AuthOnly and Capture transactions are essentially a sale process in two steps.

The sale is considered complete when you successfully complete both the Capture and AuthOnly transactions. The money will be debited from the card holder’s account and credited to your account after the sale is complete.

When the AuthOnly transaction is successfully executed, you will receive a 201 Created code on a successful AuthOnly. This code also includes the transaction ID. You can use this transaction ID to run the Capture transaction.

##Request Method
`POST`

##Endpoint (URL to Call)
`POST http://payhub.com/payhubws/api/v2/authOnly`

##Elements

###merchant
Key | Type | Value
--- | ---- | -----
organization_id | integer | The Organization Id of the Merchant. <br>This value must match with the Organization Id of the Merchant with which the passed Oauth Token is associated.
terminal_id | integer | The virtual Terminal Id of the merchant for the 3rd Party API.

###bill
Key | Type | Value
--- | ---- | -----
base_amount | TransactionAmount | The Base amount of the recurring bill. <br>The total amount charged will be the sum of this amount and (any) 'shipping_amount' and 'tax_amount'.
shipping_amount | TransactionAmount, optional | The shipping amount for the transaction. <br>The value will be included in the total amount charged.
tax_amount | TransactionAmount, optional | The tax amount for the transaction. <br>The value will be included in the total amount charged.
invoice_number | string, optional | The invoice number for the transaction.
po_number | string, optional | The purchase order number for the transaction.
note | string, optional | A free format note for the transaction. The note will be read by the merchant.

###card_data
Key | Type | Value
--- | ---- | -----
tokenized_card | string, optional | This is the 16 character PayHub-specific tokenized string that represents the card number.<br>You can obtain the value for the given customer and credit card by examining the last sale, by accessing the link to the card data that was successfully created. It is safer (and recommended) method for the third party client to store and use this string in any future requests for the same customer. <br>This value is required if the 'card_number' property is not present in the request.
card_number | string, optional | This is the 16 character card number. <br>The card number is not stored by the API in plain text. The response will contain a tokenized card number that should always be used in future requests, (for the same Customer) in place of this property. <br>This value is required if the 'tokenized_card' property is not present in the request.
billing_address_1 | string, optional | The billing street address of the customer.
billing_address_2 | string, optional | The additional billing street address of the customer.
billing_city | string, optional | The billing city of the customer.
billing_state | string, optional | The billing (US) State Code (CA, WI, NY, etc.) of the customer.
billing_zip | string, optional | The billing (US) Zip Code of the customer. <br> The zip code must be either 5 digits or 5 plus four separated by a dash ('-').<br> This value is required if the Merchant has turned on the AVS flag.
card_expiry_date | string | The card expiry date in the _YYYYMM_ format.
cvv_data | string | A three or four digit CVV code on the card.

###customer
Key | Type | Value
--- | ---- | -----
first_name | string | The first name of the customer.
last_name | string | The last name of the customer.
company_name | string, optional | The company name of the customer.
job_title | string, optional | The job title of the customer.
email_address | string | The email address of the customer. <br>The email address must be syntactically correct and valid. If email address is not specified, you must provide a phone_number to identify the customer.
web_address | string, optional | The web site address of the customer. <br>The website address, if specified, must be syntactically correct and valid.
phone_number | string, optional | The phone number of the customer. <br>The phone number must be syntactically correct. For example, (415) 234 5678, or 4152345678, or (415) 234-5678. <br>If phone number is not specified, you must provide an email_address to identify the customer.
phone_ext | string, optional | The phone extension number of the customer.
phone_type | string, optional | The type of the phone the customer is using. <br>The phone type can be ['H' or 'W' or 'M']: H (Home), W (Work), M (Mobile).
record_format | string, optional | The value that indicates whether the sale has to be made over a credit card, debit card or cash payment. <br>The default value is "CREDIT_CARD". <br>Following are the accepted values:<ul><li>CASH_PAYMENT</li><li>CREDIT_CARD</li><li>DEBIT_CARD</li></ul>

##Example of JSON
```
{
"merchant" : {
    "organization_id" : 10002,
     "terminal_id" : 2
},

"bill" : {
    "base_amount" : {
          "amount" : 10.0
     },
     "shipping_amount" : {
          "amount" : 1.23
      },
     "tax_amount" : {
          "amount" : 1.00
     },
     "note" : "Send as a gift.",
     "invoice_number" : "328742",
     "po_number" : "2474-6975"
},

"card_data" : {
    "card_number" : "5466410004374507",
    "card_expiry_date" : "202011",
    "billing_address_1" : "2350 Kerner Blvd",
    "billing_address_2" : "2 floor",
    "billing_city" : "San Rafael",
    "billing_state" : "CA",
    "billing_zip" : "94901",
    "cvv_data" : "998"
},

"customer" : {
    "first_name" : "Miguel",
     "last_name" : "Smith",
     "company_name" : "Payhub Inc",
     "job_title" : "Software Engineer",
     "email_address" : "msmith14567@payhub.com",
     "web_address" : "http://payhub.com",
     "phone_number" : "(415) 479 1349",
     "phone_ext" : "123",
      "phone_type" : "M"
},
"record_format" : "CREDIT_CARD"
}
```
##Result
* 201 code (created)
* The Id of the AuthOnly in the Location header <br>Sample header in the response:Location: http://payhub.com/payhubws/api/v2/authOnly/234

> You will need to use Oauth token in the header request for sales or any other transaction. For more information, see the [OAUTH]https://payhub.atlassian.net/wiki/display/MM/OAuth+2.0+Access+Tokens section.
