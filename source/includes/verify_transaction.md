#Verify Transaction

This topic provides the information about the using Verify transaction. Run the Verify transaction when you want to know if the card data you have is valid and ready to run other transactions, such as sale. The amount refunded is the same as the total amount that was originally charged in the sale. The amount is refunded to the credit or debit card that was originally charged.

#Request Method
`POST`

#Endpoint (URL to Call)
`http://payhub.com/payhubws/api/v2/verify`

#Elements

###merchant

Key | Type | Description
--- | ---- | -----
organization_id	| integer | The Organization Id of the Merchant. This must match the Organization Id of the Merchant that the passed Oauth Token is associated with.
terminal_id | integer | The merchant's virtual terminal Id for 3rd Party API.

###card_data

Key | Type | Description
--- | ---- | -----
tokenized_card	| string, optional | This is the 16 character PayHub-specific tokenized string representing the card number. <br>The value for the given customer and credit card can be obtained by examining the last Sale by accessing the link to the card data that was successfully created. It is safer (and recommended) for the third party client to store and use this string in any future requests for the same customer. <br>This value is required if the 'card_number' property is not present in the request.
card_number  | string, optional | This is the 16 character card number. <br>This value is required if the 'tokenized_card' property is not present in the request.<br>**Note:** The card number is not stored by the API in plain text. The response will contain a tokenized card number that should always be used in future requests, (for the same Customer) in place of this property.
track1_data  | string, optional | This is the string read by the swiper (It contents track 1 data without sentinels).
track2_data  | string, optional | This is the string read by the swiper (It contents track 2 data without sentinels).
encrypted_track_data | optional |<ul><li>**encrypted_track** (string, optional): This is the string read by the swiper. This string contains the encrypted track data.</li><li>**swiper_brand** (string, optional): The swiper brand's name with capital letters. Default value is **IDTECH**.</li><li>**swiper_model** (string, optional): The swiper model with capital letters. Default value is **UNIMAGII**.</li></ul>
billing_address_1 | string, optional | The billing street address of the customer.
billing_address_2 | string, optional | The additional billing street address of the customer.
billing_city | string, optional | The billing city of the customer.
billing_state | string, optional | The billing state Code of the customer. For example, CA, WI, NY, etc. <br>The sate codes should be for the states in the USA.
billing_zip | string, optional | The billing Zip Code of the customer. The zip code must be either 5 digits or 5 plus four separated by a '-'. <br>The zip code is required if the merchant has turned ON the AVS flag.
card_expiry_date | string | The card expiry in the YYYYMM format.
cvv_data | string | This is the three or four digit CVV code on the credit and debit card.

##customer
Key | Type | Description
--- | ---- | -----
first_name	| string | The first name of the customer.
last_name | string | The last name of the customer.
company_name | string | The company name of the customer.
job_title | string, optional | The job title of the customer.
email_address | string, optional | The valid email address of the customer. <br>The email address must be syntactically correct. If you do not specify the email address, you must provide a phone_number to identify the customer.
web_address | string, optional | The web address of the customer. <br>The web address, if you specify, must be syntactically valid web address.
phone_number | stirng, optional | The phone number of the customer. <br> The phone number must be syntactically correct. For example, (415) 234 5678, or 4152345678, or (415) 234-5678. <br>If you do not specify the phone number, you must provide an email_address to identify the customer.
phone_ext | string, optional | The phone extension number of the customer.
phone_type | string, optional | The type (['H' or 'W' or 'M']) phone number: H (Home), W (Work), M (Mobile).


####Sample Object in JSON Format
```
{
   "merchant" : {
      "organization_id" : 10005,
      "terminal_id" : 5
   },

   "card_data" : {
      "card_number" : "5466410004374507",
      "card_expiry_date" : "202011",
      "billing_address_1" : "2350 Kerner Blvd",
      "billing_address_2" : "On the corner",
      "billing_city" : "San Rafael",
      "billing_state" : "CA",
      "billing_zip" : "94901",
      "cvv_data" : "998"
   },
   "customer" : {
      "first_name" : "Adam",
      "last_name" : "Smith",
      "company_name" : "Go Capitalism Inc",
      "job_title" : "Author",
      "email_address" : "adam@smith.com",
      "web_address" : "http://gocaptilaism.com",
      "phone_number" : "(415) 479 1349",
      "phone_ext" : "123",
      "phone_type" : "M"
   }
}
```

##Response
* A 201 Created status if the card data is correct. That's all you need to know if card data is valid.

**Note:** You will need to use Oauth token in the header request for sales or any other transaction. For more information, see the OAUTH section.
