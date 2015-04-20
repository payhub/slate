# Recurring Billing
```java
package transactions;

import java.io.IOException;
import java.io.InputStream;
import java.io.UnsupportedEncodingException;
import java.net.URL;

import org.apache.http.Header;
import org.apache.http.HttpResponse;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.params.HttpConnectionParams;
import org.apache.http.params.HttpParams;
import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

public class RecurringBill {

  public static void CreateRecurringBill() {
    try {
      HttpClient client = new DefaultHttpClient();
      HttpParams params = client.getParams();
      HttpConnectionParams.setConnectionTimeout(params, 10000);

      JSONObject jsonRequestObject = new JSONObject();
      String url = "http://payhub.com/payhubws/api/v2/recurring-bill";

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
        card_data.put("billing_zip", "10016");
        card_data.put("billing_city", "New York");
        card_data.put("billing_state", "NY");
      jsonRequestObject.put("card_data", card_data);

      JSONObject customer = new JSONObject();
        customer.put("first_name", "John");
        customer.put("last_name", "Smith");
        customer.put("company_name", "CBA Steakhouse");
        customer.put("job_title", "Owner");
        customer.put("email_address", "john@cbasteakhouse.com");
        customer.put("phone_number", "(917) 479 1349");
        customer.put("phone_ext", "5478");
        customer.put("phone_type", "M");
      jsonRequestObject.put("customer", customer);

      JSONObject schedule = new JSONObject();
        schedule.put("schedule_type", "S");
        JSONObject specific_dates_schedule = new JSONObject();
          JSONArray specific_dates = new JSONArray();
            specific_dates.put(0,"2016-02-01");
            specific_dates.put(1,"2016-10-31");
          specific_dates_schedule.put("specific_dates", specific_dates);
        schedule.put("specific_dates_schedule", specific_dates_schedule);
      jsonRequestObject.put("schedule", schedule);

      HttpPost postCreate = new HttpPost(url);
      postCreate.addHeader("Authorization", "Bearer dfd4f12a-79af-4825-8dba-db27342c8491");//You put your token here
      postCreate.addHeader("Content-Type", "application/json");
      postCreate.addHeader("Accept", "application/json");

      StringEntity se = new StringEntity(jsonRequestObject.toString());
      postCreate.setEntity(se);
      HttpResponse response = client.execute(postCreate);//You return this response and work with it

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
            System.out.println("Recurring Bill Id: " + transactionId ); //Last resource of the path
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
    "organization_id": 10005,
    "terminal_id": 5
  },
  "bill": {
    "base_amount": {
      "amount": 72.34
    },
    "shipping_amount": {
      "amount": 3.87
    },
    "tax_amount": {
      "amount": 7.23
    },
    "note": "Gym stuff monthly delivered.",
    "invoice_number": "FF7845742ARG",
    "po_number": "What is a PO number?"
  },
  "card_data": {
    "card_number": "4055011111111111",
    "card_expiry_date": "201709",
    "billing_address_1": "154 E 34th St",
    "billing_address_2": "Fifth Floor",
    "billing_city": "New York",
    "billing_state": "NY",
    "billing_zip": "10016"
  },
  "customer": {
    "first_name": "Adam",
    "last_name": "Smith",
    "company_name": "Capital Inc",
    "job_title": "President",
    "email_address": "adam@smith.com",
    "web_address": "http://capitalinc.com",
    "phone_number": "(917) 479 1349",
    "phone_ext": "5702",
    "phone_type": "W"
  },
  "schedule": {
    "schedule_type": "S",
    "specific_dates_schedule": {
      "specific_dates": [
        "2015-10-01",
        "2016-09-30"
      ]
    }
  }
}
```

The purpose of this document is to provide you with what you need to understand what you need to set up a recurring bill.

Use the recurring billing functionality when you need to charge a customer a certain amount on periodic basis. You can use this feature for subscriptions or for any ongoing product or service you offer. You can setup a recurring billing by calling our RESTful API through an HTTP post containing a JSON.

### Endpoint (URL to Call):
`POST http://payhub.com/payhubws/api/recurring-bill`

## RecurringBillingInput

```json
{
  "order": {
    "id": 1,
    "invoice":"a123",
    "lines": [
      {"item": "part 1"},
      {"item": "part 2"}
    ]
  }
}
```

Key | Value
--- | -----
customer (CustomerInput): |  Customer detail,
card_data (CardDataInput): |  Card data to use in the bill.,
merchant (MerchantInput): |  Merchant detail,
bill (BillInput): |  Bill-related data to use in the recurring bill.,
schedule (ScheduleInput): |  Schedule detail,
metaData (MetaData, optional): |  A client-specific arbitrary (but valid) JSON string to represent data in the client's domain that can be associated with this entity.

##CustomerInput

```java
JSONObject customer = new JSONObject();
  customer.put("first_name", "John");
  customer.put("last_name", "Smith");
  customer.put("company_name", "CBA Steakhouse");
  customer.put("job_title", "Owner");
  customer.put("email_address", "john@cbasteakhouse.com");
  customer.put("phone_number", "(917) 479 1349");
  customer.put("phone_ext", "5478");
  customer.put("phone_type", "M");
jsonRequestObject.put("customer", customer);
````

```json
{
  "customer": {
    "first_name": "Adam",
    "last_name": "Smith",
    "company_name": "Capital Inc",
    "job_title": "President",
    "email_address": "adam@smith.com",
    "web_address": "http://capitalinc.com",
    "phone_number": "(917) 479 1349",
    "phone_ext": "5702",
    "phone_type": "W"
  }
}
```

Key | Value
--- | -----
first_name (string): |  The customer's first name.,
last_name (string): |  The customer's last name.,
company_name (string, optional): |  The customer's company name.,
job_title (string, optional): |  The customer's job title.,
email_address (string, optional): |  The customer's email address. Must be syntactically correct. If email address is not specified, you must provide a phone_number to identify the customer.,
web_address (string, optional): |  The customer's web address. If specified, must be a syntactically valid web address,
phone_number (string, optional): |  The customer's phone number. Must be syntactically correct. Example: |  (415) 234 5678, or 4152345678, or (415) 234-5678. If phone number is not specified, you must provide an email_address to identify the customer.,
phone_ext (string, optional): |  The customer's phone extension.,
phone_type (string, optional) = ['H' or 'W' or 'M']: |  The type of the customer's phone number: |  H (Home), W (Work), M (Mobile).,
metaData (MetaData, optional): |  A client-specific arbitrary (but valid) JSON string to represent data in the client's domain that can be associated with this entity.

## MetaData

Key | Value
--- | -----

## CardDataInput

```java
JSONObject card_data = new JSONObject();
  card_data.put("card_number", "5466410004374507");
  card_data.put("card_expiry_date", "201809");// September 2018
  card_data.put("billing_address_1", "237 E 33rd Street");
  card_data.put("billing_address_2", "3rd Floor");
  card_data.put("billing_zip", "10016");
  card_data.put("billing_city", "New York");
  card_data.put("billing_state", "NY");
jsonRequestObject.put("card_data", card_data);
```

```json
{
  "card_data": {
    "card_number": "4055011111111111",
    "card_expiry_date": "201709",
    "billing_address_1": "154 E 34th St",
    "billing_address_2": "Fifth Floor",
    "billing_city": "New York",
    "billing_state": "NY",
    "billing_zip": "10016"
  }
}
```

Key | Value
--- | -----
card_expiry_date (string): |  The Card Expiry in the form YYYYMM.,
tokenized_card (string, optional): |  (Required if the 'card_number' property is not present in the request). This is the 16 character PayHub-specific tokenized string representing the card number. The value for the given customer and credit card can be obtained by examining the lastRecurringBillResponse.billingReferences.tokenizedCard by accessing the link to the bill that was successfully created. It is safer (and recommended) for the third party client to store and use this string in any future requests for the same customer.,
card_number (string, optional): |  (Required if the 'tokenized_card' property is not present in the request). This is the 16 character card number. Note that the card number is not stored by the API in plain text - the response will contain a tokenized card number that should always be used in future requests, (for the same Customer) in place of this property.,
billing_address_1 (string, optional): |  The customer's billing street address.,
billing_address_2 (string, optional): |  The customer's billing street address - line 2.,
billing_city (string, optional): |  The customer's billing city,
billing_state (string, optional): |  The customer's billing (US) State Code (CA, WI, NY, etc.), billing_zip (string, optional): |  The customer's billing (US) Zip Code. Must be either 5 digits or 5 plus four separated by a '-'. (Required if the Merchant has the AVS flag turned on.),
metaData (MetaData, optional): |  A client-specific arbitrary (but valid) JSON string to represent data in the client's domain that can be associated with this entity.

## MerchantInput

```java
JSONObject merchant = new JSONObject();
  merchant.put("organization_id", "10005");//You put your org id here
  merchant.put("terminal_id", "5");//You put your terminal id
jsonRequestObject.put("merchant", merchant);
```

```json
{
  "merchant": {
    "organization_id": 10005,
    "terminal_id": 5
  }
}
```

Key | Value
--- | -----
organization_id (integer) = ['range[0-]']: | The Organization Id of the Merchant. This must match the Organization Id of the Merchant that the passed Oauth Token is associated with.,
terminal_id (integer) = ['range[0-]']: | The Merchant's Virtual Terminal Id for 3rd Party API., metaData (MetaData, optional): A client-specific arbitrary (but valid) JSON string to represent data in the client's domain that can be associated with this entity.

## BillInput

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
      "amount": 72.34
    },
    "shipping_amount": {
      "amount": 3.87
    },
    "tax_amount": {
      "amount": 7.23
    },
    "note": "Gym stuff monthly delivered.",
    "invoice_number": "FF7845742ARG",
    "po_number": "What is a PO number?"
  }
}
```

Key | Value
--- | -----
tax_amount (TransactionAmount, optional): | The tax amount for the transaction. This will be included in the total amount charged.,
po_number (string, optional): | The purchase order number for the transaction.,
base_amount (TransactionAmount): | The Base amount of the recurring bill. The total amount charged will be the sum of this amount and (any) 'shipping_amount' and 'tax_amount'.,
shipping_amount (TransactionAmount, optional): | The shipping amount for the transaction. This will be included in the total amount charged.,
invoice_number (string, optional): | The invoice number for the transaction.,
note (string, optional): | A free format note for the transaction. The note will be readable by the Merchant.,
metaData (MetaData, optional): | A client-specific arbitrary (but valid) JSON string to represent data in the client's domain that can be associated with this entity.

## TransactionAmount

```json
{"order":
  {
    "id": 1, "invoice":"a123",
    "lines": [
      {"item": "part 1"},
      {"item": "part 2"}
    ]
  }
}
```

Key | Value
--- | -----
amount (number): | The amount expressed as a decimal number with at most two decimal places (example: 1.23),
currency (string, optional) = ['USD']: | The ISO 4217 currency code for this amount. Currently only 'USD' is accepted.

## ScheduleInput

```java
JSONObject schedule = new JSONObject();
  schedule.put("schedule_type", "S");
  JSONObject specific_dates_schedule = new JSONObject();
    JSONArray specific_dates = new JSONArray();
      specific_dates.put(0,"2016-02-01");
      specific_dates.put(1,"2016-10-31");
    specific_dates_schedule.put("specific_dates", specific_dates);
  schedule.put("specific_dates_schedule", specific_dates_schedule);
jsonRequestObject.put("schedule", schedule);
```

```json
{
  "schedule": {
    "schedule_type": "S",
    "specific_dates_schedule": {
      "specific_dates": [
        "2015-10-01",
        "2016-09-30"
      ]
    }
  }
}
```

Key | Value
--- | -----
specific_dates_schedule (SpecificDatesSchedule, optional): | Required if schedule_type is 'S' (='Specific Dates'),
weekly_schedule (WeeklySchedule, optional): | Required if schedule_type is 'W' (='Weekly'), schedule_type (string) = ['M' or 'D' or 'Y' or 'W' or 'S']: The type of schedule being set up: 'M' (='Monthly'), 'D' (='Daily'), 'Y' (='Yearly'), 'W' (='Weekly'), S=Specific Dates.,
bill_generation_interval (integer, optional): | The interval between bills where the unit is dependent on the value of the schedule_type (example: if the schedule_type is 'W' and this field is 3, the bill will recur every three weeks). Applicable to schedule_typeof 'M' (='Monthly'), 'D' (='Daily'), 'Y' (='Yearly'), and 'W' (='Weekly'),
monthly_schedule ( MonthlySchedule, optional): | Required if schedule_type is 'M' (='Monthly'),
schedule_start_and_end (ScheduleStartAndEnd, optional): | Required if schedule_type is not 'S' ( not 'Specific Dates'),
yearly_schedule (YearlySchedule, optional): | Required if schedule_type is 'Y' (='Yearly'),
metaData (MetaData, optional): | A client-specific arbitrary (but valid) JSON string to represent data in the client's domain that can be associated with this entity.

## SpecificDatesSchedule

```json
{"order":
  {
    "id": 1, "invoice":"a123",
    "lines": [
      {"item": "part 1"},
      {"item": "part 2"}
    ]
  }
}
```

Key | Value
--- | -----
specific_dates (array[string]): | An array of specific dates on which the bill should recur. The dates should be in YYYY-MM-DD format. There must be at least one date specified and all dates must be later than today's date.


## WeeklySchedule

```json
{"order":
  {
    "id": 1, "invoice":"a123",
    "lines": [
      {"item": "part 1"},
      {"item": "part 2"}
    ]
  }
}
```

Key | Value
--- | -----
weekly_bill_days (array[string]): | The day(s) of week on which the weekly bill recurs. The array must have at least one entry and the entries must each be the first three characters of the day name, as in 'SUN', and 'SAT'.

## MonthlySchedule

```json
{"order":
  {
    "id": 1, "invoice":"a123",
    "lines": [
      {"item": "part 1"},
      {"item": "part 2"}
    ]
  }
}
```

Key | Value
--- | -----
monthly_type (string) = ['O' or 'E']: | Determines the day(s) of the month on which the bill will recur. Valid values are 'O' (='On the' - also specify monthly_on_the_day_of_week_in_month and the monthly_day_of_week) and 'E' (='Each' - also specify monthly_each_days).,
monthly_each_days (array[integer], optional): | Applicable to monthly_type of 'E' (='Each'). An array of integers that must have at least one entry and the entries must each be in the range between 1 and 32 where a '1' means 'first day of month', a '2' means 'second day of month and so on. An entry of '32' means 'last day of month'.,
monthly_on_the_day_of_week_in_month (integer, optional) = ['range[1-5]']: | Applicable to monthly_type of 'O' (='On the'). A value of 1 means 'first of month', 2 means 'second of month' and so on, with a value of 5 meaning 'last of month'. Must be specified in combination with monthly_day_of_week which supplies the day of week (Monday, Tuesday, etc.) that this applies to., monthly_day_of_week (integer, optional) = ['range[1-7]']: | Applicable to monthly_type of 'O' (='On the'). The day of week on which the bill recurs, where 1 is Sunday and 7 is Saturday. Must be specified in combination with monthly_on_the_day_of_week_in_month which supplies the 'position' (First, Second, etc.) of the day in the month (First Monday, Second Friday, Last Saturday, etc.)

## ScheduleStartAndEnd

```json
{"order":
  {
    "id": 1, "invoice":"a123",
    "lines": [
      {"item": "part 1"},
      {"item": "part 2"}
    ]
  }
}
```

Key | Value
--- | -----
start_date (string, optional): | The date to start the schedule in format YYYY-MM-DD. Required if schedule_type is 'M' (='Monthly'), 'D' (='Daily'), or 'W' (='Weekly'),
end_date (string, optional): | The date to end the schedule in format YYYY-MM-DD. Applicable to end_date_type equal to 'O' (='End On date'),
end_date_type (string, optional) = ['N' or ' A' or ' O']: | Determines the date on which the schedule ends, values can be 'N' (='Never'), 'A' (='After a number of bills' - you must also specify a end_after_bill_count), or 'O' (='On or no later than a specific date' - you must also specify a end_date). If not present, then 'N' (='Never') is assumed.,
end_after_bill_count (integer, optional): | The number of bills before ending the schedule. Applicable to end_date_type equal to 'A' (='After a number of bills')


## YearlySchedule

```json
{"order":
  {
    "id": 1, "invoice":"a123",
    "lines": [
      {"item": "part 1"},
      {"item": "part 2"}
    ]
  }
}
```

Key | Value
--- | -----
year_to_start (string): | The year in which the schedule starts in format YYYY. You must also specify yearly_bill_on_month_number and yearly_bill_on_day_of_month., yearly_bill_on_month_number ( integer) = ['range[1-12]']: The month number on which the bill will recur, where 1 is January and 12 is December. Must be specified in combination with year_to_start and yearly_bill_on_day_of_month.,
yearly_bill_on_day_of_month (string) = ['range[1-31]']: | The day of the month on which the yearly bill will recur, where 1 is the 1st, 2 is the 2nd, etc. If you specify a day of month that is invalid for the month specified by yearly_bill_on_month_number, then the last day of the month is used. Must be specified in combination with year_to_start and yearly_bill_on_month_number.


## GET

> Response will look like:

```
Resources«object» {
  content (Collection«object», optional),
  links (array[Link], optional)
}

Collection«object» {
  empty (boolean, optional)
}

Link {
  templated (boolean, optional),
  rel (string, optional),
  href (string, optional)
}
```

You can also send a GET to http://payhub.com/payhubws/api/recurring-bill to list all recurring bills and associated operations in compact form.

## Response Messages

Code | Meaning
---- | -------
200 | (not returned)
201 | The recurring bill was created - use the link in the response's 'Location' Header to view the Recurring Bill.
400 | Bad request due to incorrect or invalid data passed.
401 | Unauthorized. You do not have permission to perform this operation. Make sure you entered a valid API Key at the top of the page.
403 | Forbidden. The server does not permit you to use this URI.
500 | Internal server error due to encoding the data, or to a PayHub server failure. Contact PayHub.

ResourceSupport {
links ( array[Link], optional)
}
Link {
templated ( boolean, optional), rel ( string, optional),
href ( string, optional) }
Void {
}
RestErrorStructure {
errors ( array[RestError], optional)
}
RestError {
status ( string, optional) = ['100' or '101' or '102' or '103' or '200' or '201' or '202' or '203' or '204' or '205' or '206' or '207' or '208' or '226' or '300' or '301' or '302' or '302' or '303' or '304' or '305' or '307' or '308' or '400' or '401' or '402' or '403' or '404' or '405' or '406' or '407' or '408' or '409' or '410' or '411' or '412' or '413' or '414' or '415' or '416' or '417' or '418' or '419' or '420' or '421' or '422' or '423' or '424' or '426' or '428' or '429' or '431' or '500' or '501' or '502' or '503' or '504' or '505' or '506' or '507' or '508' or '509' or '510' or '511']: The Http Status Code,
code ( string, optional), location ( string, optional), reason ( string, optional), severity ( string, optional) }
Void { 401

}
Void { }
Void { }


