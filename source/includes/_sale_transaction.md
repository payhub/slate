# Sale Transaction
```java
public class Sale {

  public static void RunSale() {
    try {
      HttpClient client = new DefaultHttpClient();
      HttpParams params = client.getParams();
      HttpConnectionParams.setConnectionTimeout(params, 10000);

      JSONObject jsonRequestObject = new JSONObject();
      String url = "http://payhub.com/payhubws/api/v2/sale";

      JSONObject merchant = new JSONObject();
        merchant.put("organization_id", "10005");//You put your org id here
        merchant.put("terminal_id", "5");//You put your terminal id
      jsonRequestObject.put("merchant", merchant);

      JSONObject bill = new JSONObject();
        JSONObject base_amount = new JSONObject();
          base_amount.put("amount", "1275");//$12.75
        bill.put("base_amount", base_amount);
        JSONObject shipping_amount = new JSONObject();
          shipping_amount.put("amount", "725");//$7.25
        bill.put("shipping_amount", shipping_amount);
        JSONObject tax_amount = new JSONObject();
          tax_amount.put("amount", "113");//$1.13
        bill.put("tax_amount", tax_amount);
        bill.put("note", "Shipped as a gift");
        bill.put("invoice_number", "4645782");
        bill.put("po_number", "1234-654321");
      jsonRequestObject.put("bill", bill);

      JSONObject card_data = new JSONObject();
        card_data.put("card_number", "5466410004374507");
        card_data.put("card_expiry_date", "201809");// September 2018
        card_data.put("billing_address_1", "237 E 33rd Street");
        card_data.put("billing_address_2", "3rd Floor");
        card_data.put("billing_city", "New York");
        card_data.put("billing_zip", "10016");
        card_data.put("billing_state", "NY");
        card_data.put("cvv_data", "998");
        card_data.put("cvv_code", "Y");
      jsonRequestObject.put("card_data", card_data);

      JSONObject customer = new JSONObject();
        customer.put("first_name", "John");
        customer.put("last_name", "Smith");
        customer.put("company_name", "CBA Steakhouse");
        customer.put("job_title", "Owner");
        customer.put("email_address", "john@cbasteakhouse.com");
        customer.put("web_address", "http://www.cbasteakhouse.com");
        customer.put("phone_number", "(917) 479 1349");
        customer.put("phone_ext", "5478");
        customer.put("phone_type", "M");
      jsonRequestObject.put("customer", customer);

      jsonRequestObject.put("record_format", "CC");

      HttpPost postCreate = new HttpPost(url);
      postCreate.addHeader("Authorization", "Bearer dfd4f12a-79af-4825-8dba-db27342c8491");//You put your token here
      postCreate.addHeader("Content-Type", "application/json");
      postCreate.addHeader("Accept", "application/json");

      StringEntity se = new StringEntity(jsonRequestObject.toString());
      postCreate.setEntity(se);
      HttpResponse response = client.execute(postCreate); //You return this response and work with it

      String json = " ";
      JSONObject jsonResponseObject = null;

      InputStream in = response.getEntity().getContent();
      int cnt = 0;
      while ((cnt = in.read()) > -1) {
        json += (char) cnt;
      }
      if (json.charAt(0) != '<') {
        jsonResponseObject = new JSONObject(
            (json.equalsIgnoreCase("")
                || json.equalsIgnoreCase(" ") ? "{\"MESSAGE\":\"NO RESPONSE...\"}"
                : json));
      }
      String result = response.getStatusLine().getReasonPhrase();
      System.out.println(result);
      if (result.equals("Created")){
        Header[] headers = response.getAllHeaders();
        for (Header header : headers) {
          if (header.getName().equals("Location")){
            URL location = new URL(header.getValue());
            String path = location.getPath();
            int lastSlash = path.lastIndexOf("/");
            String transactionId = path.substring(lastSlash+1);
            System.out.println("Transaction Id: " + transactionId ); //Last resource of the path
          }

        }
      }
      else{
        System.out.println(jsonResponseObject.toString());
      }


    } catch (JSONException e){
      System.out.println(e.getMessage());
    } catch (UnsupportedEncodingException e) {
      System.out.println(e.getMessage());
    } catch (ClientProtocolException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
  }

}
```

```json
{
  "merchant": {
    "organization_id": 10002,
    "terminal_id": 2
  },
  "bill": {
    "base_amount": {
      "amount": 10.0
    },
    "shipping_amount": {
      "amount": 1.23
    },
    "tax_amount": {
      "amount": 1.00
    },
    "note": "This is a note.",
    "invoice_number": "328",
    "po_number": "sample po number"
  },
  "card_data": {
    "card_number": "5466410004374507",
    "card_expiry_date": "202011",
    "billing_address_1": "2350 Kerner Blvd",
    "billing_address_2": "2 floor",
    "billing_city": "San Rafael",
    "billing_state": "CA",
    "billing_zip": "94901",
    "cvv_data": "998"
  },
  "customer": {
    "first_name": "Miguel",
    "last_name": "Smith",
    "company_name": "Payhub Inc",
    "job_title": "Software Engineer",
    "email_address": "msmith14567@payhub.com",
    "web_address": "http://payhub.com",
    "phone_number": "(415) 479 1349",
    "phone_ext": "123",
    "phone_type": "M"
  }
}
```

The purpose of this document is to provide you with what you need to understand what you need to run a sale transaction.

Use a Sale transaction to charge a credit card. You will receive an approval code on a successful sale. Once a sale transaction is settled, the money will be taken from the cardholder's account and deposited to the merchant's account. This is the most common transaction type by far.

### Endpoint (URL to Call):
`POST http://payhub.com/payhubws/api/sale`

As a result you'll receive a 201 code (created) and the Id of the Sale in the Location header. Sample header in the response:

`Location → http:// [payhub-api-server]:8251/payhubws/api/sale/5501b651da06a879ce520d4d`

If you do a GET request to this URL, you will get all the transaction information in JSON format.

## merchant

```java
JSONObject merchant = new JSONObject();
  merchant.put("organization_id", "10005");//You put your org id here
  merchant.put("terminal_id", "5");//You put your terminal id
jsonRequestObject.put("merchant", merchant);
```

```json
{
  "merchant": {
    "organization_id": 10002,
    "terminal_id": 2
  }
}
```

Key | Value
--- | -----
organization_id (integer): | The Organization Id of the Merchant. This must match the Organization Id of the Merchant that the passed Oauth Token is associated with.,
terminal_id (integer): | The Merchant's Virtual Terminal Id for 3rd Party API.

## bill

```java
JSONObject bill = new JSONObject();
  JSONObject base_amount = new JSONObject();
    base_amount.put("amount", "1275");//$12.75
  bill.put("base_amount", base_amount);
  JSONObject shipping_amount = new JSONObject();
    shipping_amount.put("amount", "725");//$7.25
  bill.put("shipping_amount", shipping_amount);
  JSONObject tax_amount = new JSONObject();
    tax_amount.put("amount", "113");//$1.13
  bill.put("tax_amount", tax_amount);
  bill.put("note", "Shipped as a gift");
  bill.put("invoice_number", "4645782");
  bill.put("po_number", "1234-654321");
jsonRequestObject.put("bill", bill);
```

```json
{
  "bill": {
    "base_amount": {
      "amount": 10.0
    },
    "shipping_amount": {
      "amount": 1.23
    },
    "tax_amount": {
      "amount": 1.00
    },
    "note": "This is a note.",
    "invoice_number": "328",
    "po_number": "sample po number"
  }
}
```

Key | Value
--- | -----
base_amount (TransactionAmount): | The Base amount of the recurring bill. The total amount charged will be the sum of this amount and (any) 'shipping_amount' and 'tax_amount'.
shipping_amount (TransactionAmount, optional): | The shipping amount for the transaction. This will be included in the total amount charged.,
tax_amount (TransactionAmount, optional): | The tax amount for the transaction. This will be included in the total amount charged.,
invoice_number (string, optional): | The invoice number for the transaction., po_number (string, optional): | The purchase order number for the transaction.
note (string, optional): | A free format note for the transaction. The note will be readable by the Merchant.

## card_data

```java
JSONObject card_data = new JSONObject();
  card_data.put("card_number", "5466410004374507");
  card_data.put("card_expiry_date", "201809");// September 2018
  card_data.put("billing_address_1", "237 E 33rd Street");
  card_data.put("billing_address_2", "3rd Floor");
  card_data.put("billing_city", "New York");
  card_data.put("billing_zip", "10016");
  card_data.put("billing_state", "NY");
  card_data.put("cvv_data", "998");
  card_data.put("cvv_code", "Y");
jsonRequestObject.put("card_data", card_data);
```

```json
{
  "card_data": {
    "card_number": "5466410004374507",
    "card_expiry_date": "202011",
    "billing_address_1": "2350 Kerner Blvd",
    "billing_address_2": "2 floor",
    "billing_city": "San Rafael",
    "billing_state": "CA",
    "billing_zip": "94901",
    "cvv_data": "998"
  }
}
```

Key | Value
--- | -----
tokenized_card (string, optional): | (Required if the 'card_number' property is not present in the request). This is the 16 character PayHub-specific tokenized string representing the card number. The value for the given customer and credit card can be obtained by examining the last Sale by accessing the link to the card data that was successfully created. It is safer (and recommended) for the third party client to store and use this string in any future requests for the same customer. card_number ( string, optional): (Required if the 'tokenized_card' property is not present in the request). This is the 16 character card number. Note that the card number is not stored by the API in plain text - the response will contain a tokenized card number that should always be used in future requests, (for the same Customer) in place of this property.,
billing_address_1 (string, optional): | The customer's billing street address.
billing_address_2 ( tring, optional): | The customer's billing street address - line 2.,
billing_city (string, optional): | The customer's billing city,
billing_state (string, optional): | The customer's billing (US) State Code (CA, WI, NY, etc.), billing_zip ( string, optional): The customer's billing (US) Zip Code. Must be either 5 digits or 5 plus four separated by a '-'. (Required if the Merchant has the AVS flag turned on.),
card_expiry_date (string): | The Card Expiry in the form YYYYMM., cvv_data ( string): This is the three or four digit card CVV code.

## customer
```java
JSONObject customer = new JSONObject();
  customer.put("first_name", "John");
  customer.put("last_name", "Smith");
  customer.put("company_name", "CBA Steakhouse");
  customer.put("job_title", "Owner");
  customer.put("email_address", "john@cbasteakhouse.com");
  customer.put("web_address", "http://www.cbasteakhouse.com");
  customer.put("phone_number", "(917) 479 1349");
  customer.put("phone_ext", "5478");
  customer.put("phone_type", "M");
jsonRequestObject.put("customer", customer);
```

```json
{
  "customer": {
    "first_name": "Miguel",
    "last_name": "Smith",
    "company_name": "Payhub Inc",
    "job_title": "Software Engineer",
    "email_address": "msmith14567@payhub.com",
    "web_address": "http://payhub.com",
    "phone_number": "(415) 479 1349",
    "phone_ext": "123",
    "phone_type": "M"
  }
}
```

Key | Value
--- | -----
first_name (string): | The customer's first name.
last_name (string): | The customer's last name.
company_name (string, optional): | The customer's company name.
job_title (string, optional): | The customer's job title.
email_address (string, optional): | The customer's email address. Must be syntactically correct. If email address is not specified, you must provide a phone_number to identify the customer.
web_address (string, optional): | The customer's web address. If specified, must be a syntactically valid web address.
phone_number (string, optional): | The customer's phone number. Must be syntactically correct. Example: (415) 234 5678, or 4152345678, or (415) 234-5678. If phone number is not specified, you must provide an email_address to identify the customer.
phone_ext (string, optional): | The customer's phone extension.
phone_type (string, optional) = ['H' or 'W' or 'M']: | The type of the customer's phone number: H (Home), W (Work), M (Mobile).
