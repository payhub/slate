
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
import org.json.JSONException;
import org.json.JSONObject;

public class Sale {

  public static void RunVerify() {
    try {
      HttpClient client = new DefaultHttpClient();
      HttpParams params = client.getParams();
      HttpConnectionParams.setConnectionTimeout(params, 10000);

      JSONObject jsonRequestObject = new JSONObject();
      String url = "http://payhub.com/payhubws/api/v2/verify";

      JSONObject merchant = new JSONObject();
        merchant.put("organization_id", "10005");//You put your org id here
        merchant.put("terminal_id", "5");//You put your terminal id
      jsonRequestObject.put("merchant", merchant);

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

```php
<?php
  $trans_type = "verify";
  $processed = FALSE;
  $ERROR_MESSAGE = '';

  //Defining the Web Service URL
  $WsURL="http://payhub.com/payhubws/api/v2/$trans_type";


  //Defining data for the SALE transaction
  // Merchant data (obtained from the payHub Virtual Terminal (3rd party integration)
  $organization_id = 10002;
  $terminal_id = 2;
  $oauth_token = "22fe3c69-db70-4a8c-9aed-9a33ebb1e9b4";
 
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
  $web_address = "http://payhub.com";
  $phone_number = "(415) 479 1349";
  $phone_ext = "123";
  $phone_type = "M";


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
  $data ["customer"]=  array("first_name" => "$first_name","last_name" => "$last_name","company_name" => "$company_name",
    "job_title" => "$job_title","email_address" => "$email_address","web_address" => "$web_address",
    "phone_number" => "$phone_number","phone_ext" => "$phone_ext","phone_type" => "$phone_type");

  //Convert data from Array to JSON
  $data_string = json_encode($data);

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

  //Obtain the data from the verify recently created
  if ($httpcode==201){
    //find the url of the sale (Location header in the response)
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
    public class Verify
    {
        public static void RunVerify()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("http://payhub.com/payhubws/api/v2/verify");
                request.ContentType = "text/json";
                request.Method = "POST";

                var sale = new
                {
                    merchant = new
                     {
                         organization_id = "10005",
                         terminal_id = "5",
                     },                    
                    card_data = new
                    {
                        card_number = "5466410004374507",
                        card_expiry_date = "201809", //September 2018
                        billing_address_1 = "237 E 33rd Street",
                        billing_address_2 = "3rd Floor",
                        billing_city = "New York",
                        billing_state = "NY",
                        billing_zip = "10016",
                        cvv_data = "998",
                        cvv_code = "Y"
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
                    record_format = "CC"
                };

                string json = JsonConvert.SerializeObject(sale);

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
                        string result = reader.ReadToEnd();
                        Console.WriteLine(response.StatusCode);//Created
                        for (int i = 0; i < response.Headers.Count; ++i)
                        {
                            if (response.Headers.Keys[i] == "Location")
                            {
                                string path = response.Headers[i];
                                int lastSlash = path.LastIndexOf("/");
                                string transactionId = path.Substring(lastSlash + 1);
                                Console.WriteLine("Transaction Id: " + transactionId);
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

```ruby

    require 'uri'
    require 'net/http'
    require 'json'
    
    url = URI("http://payhub.com/payhubws/api/v2/verify")
    
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
    customer = {"first_name"=>"First",
                "last_name"=>"Contact",
                "company_name"=>"Payhub Inc",
                "job_title"=>"Software Engineer",
                "email_address"=>"jhon@company.com",
                "web_address"=>"http://payhub.com",
                "phone_number"=>"(415) 479 1349",
                "phone_ext"=>"123",
                "phone_type"=>"M"
    }
    
    
    informationToSend = {"merchant"=>merchant,"customer"=>customer,"card_data"=>card_data}
    
    request.body = JSON.generate(informationToSend)
    
    response = http.request(request)
    puts response.read_body
```
