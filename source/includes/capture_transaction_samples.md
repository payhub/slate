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


public class Void {

  public static void RunCapture() {
    try {
      HttpClient client = new DefaultHttpClient();
      HttpParams params = client.getParams();
      HttpConnectionParams.setConnectionTimeout(params, 10000);

      JSONObject jsonRequestObject = new JSONObject();
      String url = "http://payhub.com/payhubws/api/v2/capture";

      JSONObject merchant = new JSONObject();
      merchant.put("organization_id", "10005"); //You put your org id here
      merchant.put("terminal_id", "5");//You put your terminal id

      jsonRequestObject.put("merchant", merchant);
      jsonRequestObject.put("transaction_id", "223"); //You put the transaction id of the sale or refund you want to void

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
            System.out.println("Transaction Id: " + transactionId );
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
      $trans_type = "capture";
      $processed = FALSE;
      $ERROR_MESSAGE = '';
    
      //Defining the Web Service URL
      $WsURL="http://payhub.com/payhubws/api/v2/$trans_type";
    
    
      //Defining data for the VOID transaction
      // Merchant data (obtained from the payHub Virtual Terminal (3rd party integration)
      $organization_id = 10002;
      $terminal_id = 2;
      $oauth_token = "22fe3c69-db70-4a8c-9aed-9a33ebb1e9b4";
      $transaction_id="83";
      
      // bill data
        $base_amount = 10.0;
        $shipping_amount = 1.23;
        $tax_amount = 1.00;
        $note = "this a sample note";
        $invoice_number = "a-00240";
        $po_number = "56";
    
    
      //Convert data to array to send it to the WS as JSON format
      $data = array();
      $data["merchant"] = array("organization_id" => "$organization_id", "terminal_id" => "$terminal_id");
      $data["transaction_id"]=$transaction_id;
      $data["bill"] = array ("base_amount" => array ("currency" => "USD","amount" => $base_amount),
            "shipping_amount" => array ("currency" => "USD","amount" => $shipping_amount),
            "tax_amount" => array ("currency" => "USD","amount" => $tax_amount));
            
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
    
      //Obtain the data from the void recently created
      if ($httpcode==201){
        //find the url of the void (Location header in the response)
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
    
    using System;
    using System.IO;
    using System.Net;
    using Newtonsoft.Json;
    
    namespace PayHubSamples
    {
        public class Void
        {
            public static void RunCapture()
            {
                try
                {
                    var request = (HttpWebRequest)WebRequest.Create("http://payhub.com/payhubws/api/v2/capture");
                    request.ContentType = "text/json";
                    request.Method = "POST";
    
                    var capture = new
                    {
                        merchant = new
                        {
                            organization_id = "10005",
                            terminal_id = "5",
                        },
                        transaction_id = "238",
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
                        }
                    };
    
                    string json = JsonConvert.SerializeObject(capture);
    
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
    
         "transaction_id":"365",
    
         "bill" : {
    
             "base_amount" : {
                 "amount" : 6.00
             },
             "shipping_amount" : {
                 "amount" : 1.5
             },
             "tax_amount" : {
                 "amount" : 0.5
             },
             "note" : "Nice guest, giving him 20% discount.",
             "invoice_number" : "789468",
             "po_number" : "789468-CAP"
         }
    }
```

```ruby

    require 'uri'
    require 'net/http'
    require 'json'
    
    url = URI("http://payhub.com/payhubws/api/v2/capture")
    
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
    #transaction Id
    transaction_id="114"
    
    informationToSend = {"merchant"=>merchant,"bill"=>bill,"transaction_id"=>transaction_id}
    
    request.body = JSON.generate(informationToSend)
    
    response = http.request(request)
    puts response.read_body
```