## Void Transaction

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

  public static void RunVoid() {
    try {
      HttpClient client = new DefaultHttpClient();
      HttpParams params = client.getParams();
      HttpConnectionParams.setConnectionTimeout(params, 10000);

      JSONObject jsonRequestObject = new JSONObject();
      String url = "http://payhub.com/payhubws/api/v2/void";

      JSONObject merchant = new JSONObject();
      merchant.put("organization_id", "10005"); //You put your org id here
      merchant.put("terminal_id", "5");//You put your terminal id

      jsonRequestObject.put("merchant", merchant);
      jsonRequestObject.put("transaction_id", "223"); //You put the transaction id of the sale or refund you want to void

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
  $trans_type = "void";
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



  //Convert data to array to send it to the WS as JSON format
  $data = array();
  $data["merchant"] = array("organization_id" => "$organization_id", "terminal_id" => "$terminal_id");
  $data["transaction_id"]=$transaction_id;

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
        public static void RunVoid()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("http://payhub.com/payhubws/api/v2/void");
                request.ContentType = "text/json";
                request.Method = "POST";

                var refund = new
                {
                    merchant = new
                    {
                        organization_id = "10005",
                        terminal_id = "5",
                    },
                    transaction_id = "238"
                };

                string json = JsonConvert.SerializeObject(refund);

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
  "merchant": {
    "organization_id": 10005,
    "terminal_id": 5
  },
  "transaction_id": "114"
}
```

#Void Transaction

Run a Void when you need to cancel a transaction (either a sale or a refund) that has not yet been settled and that has not already been voided. This will avoid the customer being charged any amount at all and will release the pending funds, given the issuer supports doing so.

## Endpoint (URL to Call):
`POST http://payhub.com/payhubws/api/void`

## Elements (All required)

Key | Type | Value
--- | ---- | -----
organization_id | integer | The merchant's organization id which did the sale you're trying to void.
terminal_id | integer | The merchant's terminal's id where the sale was done.
transaction_id | integer | The transaction id of the sale that you want to void.

##Example of JSON
```
{
"merchant" : {
 "organization_id" : 10005,
 "terminal_id" : 5

 },

 "transaction_id":"114"
}
```
