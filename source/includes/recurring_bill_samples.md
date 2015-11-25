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
      String url = "https://api.payhub.com/api/v2/recurring-bill";

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


```php

<?php
  $processed = FALSE;
  $ERROR_MESSAGE = '';

  //Defining the Web Service URL
  $WsURL="https://api.payhub.com/api/v2/$trans_type";

  //Defining data for the RECURRING BILL transaction
  // Merchant data (obtained from the payHub Virtual Terminal (3rd party integration)
  $organization_id = 10002;
  $terminal_id = 2;
  $oauth_token = "22fe3c69-db70-4a8c-9aed-9a33ebb1e9b4";
  // bill data
  $base_amount = 10.0;
  $shipping_amount = 1.23;
  $tax_amount = 1.00;
  $note = "this a sample note";
  $invoice_number = "a-00240";
  $po_number = "56";
  //Credit card data
  $card_number = "5466410004374507";
  $card_expiry_date = "202011";
  $cvv_data = "998";
  $billing_address_1 = "2350 Kerner Blvd";
  $billing_address_2 = "On the corner";
  $billing_city = "San Rafael";
  $billing_state = "CA";
  $billing_zip = "94901";
  // Customer data
  $first_name = "First";
  $last_name = "Contact";
  $company_name = "Payhub Inc";
  $job_title = "Software Engineer";
  $email_address = "jhon@company.com";
  $web_address = "https://payhub.com";
  $phone_number = "(415) 479 1349";
  $phone_ext = "123";
  $phone_type = "M";
  // Schedule data
  $schedule_type = "M";
  $bill_generation_interval="1";
  //Schedule - schedule_start_and_end (object)
  $start_date="2015-06-01";
  $end_date_type="A";
  $end_after_bill_count="6";
  //Schedule - monthly_schedule (object)
  $monthly_type="E";
  //This is an array, put the elements separated by ","
  $monthly_each_days="1";

  //Convert data to array to send it to the WS as JSON format
  $data = array();
  $data["merchant"] = array("organization_id" => "$organization_id", "terminal_id" => "$terminal_id");
  $data["bill"] = array ("base_amount" => array ("currency" => "USD","amount" => $base_amount),
    "shipping_amount" => array ("currency" => "USD","amount" => $shipping_amount),
    "tax_amount" => array ("currency" => "USD","amount" => $tax_amount));
  $data["card_data"] = array("card_number" => "$card_number","card_expiry_date" => "$card_expiry_date",
    "cvv_data" => "$cvv_data","billing_address_1" => "$billing_address_1",
    "billing_address_2" => "$billing_address_2","billing_city" => "$billing_city",
    "billing_state" => "$billing_state","billing_zip" => "$billing_zip");
  $data["customer"]=  array("first_name" => "$first_name","last_name" => "$last_name","company_name" => "$company_name",
    "job_title" => "$job_title","email_address" => "$email_address","web_address" => "$web_address",
    "phone_number" => "$phone_number","phone_ext" => "$phone_ext","phone_type" => "$phone_type");
  $data["schedule"] = array("schedule_type" => $schedule_type, "bill_generation_interval" => $bill_generation_interval,
    "schedule_start_and_end" => array ("start_date" => $start_date, "end_date_type" => $end_date_type, "end_after_bill_count" => $end_after_bill_count),
    "monthly_schedule" => array ("monthly_type" => $monthly_type));
  $data["schedule"]["monthly_schedule"]["monthly_each_days"]=explode(",",$monthly_each_days);

  //Convert data from Array to JSON
  echo $data_string = json_encode($data);

  //Creating a CURL object to access the WS
  $ch = curl_init();
  //Setting the address to connect to
  curl_setopt($ch, CURLOPT_URL, $WsURL);
  //Setting HTTP method. For a new transaction we need to use POST
  curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
  //Setting data in JSON format
  curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
  //Setting the proper header.
  //Setting the oauth_token to access the WS with the proper authorization
  curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    'Content-Type: application/json',
    'Accept: application/json',
    'Authorization: Bearer '.$oauth_token)
    );
  //Store the response as a variable and not showing the content as echo $variable
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
  //Set to return the response header, this is to analyze the result and the transaction ID
  curl_setopt($ch, CURLOPT_HEADER, true);

  //execute connection to the Web Service
  $response = curl_exec($ch);
  // get some data from the response
  $httpcode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
  $header_size = curl_getinfo($ch, CURLINFO_HEADER_SIZE);
  //Get the header as string.
  $header = substr($response, 0, $header_size);
  //close connection to the Web Service
  curl_close($ch);

  //Obtain the data from the recurring bill recently created
  if ($httpcode==201){
    //find the url of the recurring bill (Location header in the response)
    preg_match("!\r\n(?:Location): *(.*?) *\r\n!", $header, $matches);
    //$url contains the URL to GET the data from the Web Service
    $url = $matches[1];
    //Once we get the transaction ID we will query for the information of the last transaction
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    //Setting HTTP method. To get a transaction response we need to use GET
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
    //Setting the proper header.
    //Setting the oauth_token to access the WS with the proper authorization
    curl_setopt($ch, CURLOPT_HTTPHEADER, array('Authorization: Bearer '.$oauth_token));
    //Store the response as a variable and not showing the content as echo $variable
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    //execute connection to the Web Service
    $response_json=curl_exec($ch);
    //close connection to the Web Service
    curl_close($ch);
    //show result (standard JSON), parse it, process it, etc..
    echo $response_json;
    // now you could parse the json object to array if you preffer: $obj = json_decode($response_json);
    $array = json_decode($response_json);
    echo "<pre>";
    print_r($array);
    echo "</pre>";
  }
  //There was an error with the WS
  else{
    echo "Error creating the ".strtoupper($trans_type).": RESPONSE MESSAGE";
    echo "<pre>";
    echo $response;
    echo "</pre>";
  }
?>
```

```csharp
using Newtonsoft.Json;
using System;
using System.IO;
using System.Net;

namespace PayHubSamples
{
    public class RecurringBilling
    {
        public static void CreateRecurringBilling()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("https://api.payhub.com/api/v2/recurring-bill");
                request.ContentType = "text/json";
                request.Method = "POST";

                var recurringBill = new
                {
                    merchant = new
                     {
                         organization_id = "10005",
                         terminal_id = "5",
                     },
                    bill = new
                    {
                        base_amount = new
                        {
                            amount = "1275" //$12.75
                        },
                        shipping_amount = new
                        {
                            amount = "725" //$7.25
                        },
                        tax_amount = new
                        {
                            amount = "113" //$1.13
                        },
                        note = "Shipped as a gift",
                        invoice_number = "4645782",
                        po_number = "1234-654321"
                    },
                    card_data = new
                    {
                        card_number = "5466410004374507",
                        card_expiry_date = "201809", //September 2018
                        billing_address_1 = "237 E 33rd Street",
                        billing_address_2 = "3rd Floor",
                        billing_city = "New York",
                        billing_state = "NY",
                        billing_zip = "10016"
                    },
                    customer = new
                    {
                        first_name = "John",
                        last_name = "Smith",
                        company_name = "CBA Stakehouse",
                        job_title = "Owner",
                        email_address = "john@cbastakehouse.com",
                        web_address = "http://www.cbasteakhouse.com",
                        phone_number = "(917) 479 1349",
                        phone_ext = "5478",
                        phone_type = "M"
                    },
                    schedule = new
                    {
                        schedule_type = "S",
                        specific_dates_schedule = new
                        {
                            specific_dates = new[]
                            {
                                "2016-02-01",
                                "2016-10-31"
                            }
                        }

                    }
                };

                string json = JsonConvert.SerializeObject(recurringBill);

                request.Headers.Add("Authorization", "Bearer dfd4f12a-79af-4825-8dba-db27342c8491");
                request.ContentType = "application/json";
                request.Accept = "application/json";

                using (var streamWriter = new StreamWriter(request.GetRequestStream()))
                {
                    streamWriter.Write(json);
                    streamWriter.Flush();
                    streamWriter.Close();
                }

                try
                {
                    var response = (HttpWebResponse)request.GetResponse();//You return this response.
                    using (var reader = new StreamReader(response.GetResponseStream()))
                    {
                        Console.WriteLine(response.StatusCode);//Created
                        for (int i = 0; i < response.Headers.Count; ++i)
                        {
                            if (response.Headers.Keys[i] == "Location")
                            {
                                string path = response.Headers[i];
                                int lastSlash = path.LastIndexOf("/");
                                string transactionId = path.Substring(lastSlash + 1);
                                Console.WriteLine("Recurring Billing Id: " + transactionId);
                            }
                        }
                    }
                }
                catch (WebException wex)
                {
                    if (wex.Response != null)
                    {
                        using (var errorResponse = (HttpWebResponse)wex.Response)//You return wex.Response instead
                        {
                            using (var reader = new StreamReader(errorResponse.GetResponseStream()))
                            {
                                string result = reader.ReadToEnd();
                                Console.WriteLine(result);
                            }
                        }
                    }
                }
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
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
```ruby

    require 'uri'
    require 'net/http'
    require 'json'
    
    url = URI("https://api.payhub.com/api/v2/authonly")
    
    http = Net::HTTP.new(url.host, url.port)
    
    request = Net::HTTP::Post.new(url)
    request["content-type"] = 'application/json'
    request["accept"] = 'application/json'
    request["authorization"] = 'Bearer 2a5d6a73-d294-4fba-bfba-957a4948d4a3'
    request["cache-control"] = 'no-cache'
    
    
    
    merchant = {
                "organization_id"=>10074,
                "terminal_id"=>134
                }
    
    # bill data
    bill= {
            "base_amount"=>"1.00",
            "shipping_amount"=>"1.00",
            "tax_amount"=>"1.00",
            "note"=>"this a sample note",
            "invoice_number"=>"this is an invoice",
            "po_number"=>"a test po number"
            }
    #Credit card data
    card_data = {
                "card_number"=>"4055011111111111",
                "card_expiry_date"=>"202012",
                "cvv_data"=>"999",
                "billing_address_1"=>"2350 Kerner Blvd",
                "billing_address_2"=>"On the corner",
                "billing_city"=>"San Rafael",
                "billing_state"=>"CA",
                "billing_zip"=>"99997-0003"
                }
    # Customer data
    customer = {
                "first_name"=>"First",
                "last_name"=>"Contact",
                "company_name"=>"Payhub Inc",
                "job_title"=>"Software Engineer",
                "email_address"=>"jhon@company.com",
                "web_address"=>"http://payhub.com",
                "phone_number"=>"(415) 479 1349",
                "phone_ext"=>"123",
                "phone_type"=>"M"
                }
    
    
    informationToSend = {"merchant"=>merchant,"bill"=>bill,"customer"=>customer,"card_data"=>card_data}
    
    request.body = JSON.generate(informationToSend)
    
    response = http.request(request)
    puts response.read_body
```