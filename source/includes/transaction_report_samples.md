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

public class Transactions {

  public static void findTransactions() {
    try {
      HttpClient client = new DefaultHttpClient();
      HttpParams params = client.getParams();
      HttpConnectionParams.setConnectionTimeout(params, 10000);

      JSONObject jsonRequestObject = new JSONObject();
      String url = "http://payhub.com/payhubws/api/v2/report/transactionReport";

      jsonRequestObject.put("batchIdFrom", "5");
      jsonRequestObject.put("batchIdTo", "10");
      jsonRequestObject.put("transactionType", "Sale");
      jsonRequestObject.put("responseCode", "00");
      jsonRequestObject.put("amountFrom", "1");
      jsonRequestObject.put("amountTo", "2");
      jsonRequestObject.put("firstName", "First");
      jsonRequestObject.put("lastName", "Contact");
      jsonRequestObject.put("phoneNumber", "(415) 479 1349");
      jsonRequestObject.put("email", "jhon@company.com");
      jsonRequestObject.put("trnDateFrom", "2015-06-06 00:00:00");
      jsonRequestObject.put("trnDateTo", "2015-07-07 23:59:59");
      jsonRequestObject.put("cardType", "Visa");
                        

      HttpPost postCreate = new HttpPost(url);
      postCreate.addHeader("Authorization", "Bearer dfd4f12a-79af-4825-8dba-db27342c8491");//You put your token here
      postCreate.addHeader("Content-Type", "application/json");
      postCreate.addHeader("Accept", "application/json");

      StringEntity se = new StringEntity(jsonRequestObject.toString());
      postCreate.setEntity(se);
      HttpResponse response = client.execute(postCreate); //You return this response and work with it

      String json = " ";
      JSONObject jsonResponseObject = null;
      int statusCode = responseDataRequest.getResponseCode();
      InputStream in = response.getEntity().getContent();
      if (statusCode >= 200 && statusCode < 400) {
      			BufferedReader in = new BufferedReader(new InputStreamReader(responseDataRequest.getInputStream()));
              	String line;
              	while ((line = in.readLine()) != null){ 
              		response.append(line); 
              	}
              		in.close();
                      System.out.println(response.toString());   	
      		}else{
      			BufferedReader er = new BufferedReader(new InputStreamReader(responseDataRequest.getErrorStream()));
               	String line;
               	try {
               		int c = 0;
      			     while((c = er.read()) != -1) {					         
      			          response.append((char)c);
      			     }					    
      						 
      				} catch (IOException e) {
      					// TODO Auto-generated catch block
      					e.printStackTrace();
      				}
                  System.out.println(response.toString());
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
  $WsURL="http://payhub.com/payhubws/api/v2/report/transactionReport";


  $batchIdFrom= "5";
  $batchIdTo= "10";
  $transactionType= "Sale";
  $responseCode= "00";
  $amountFrom= "1";
  $amountTo= "2";
  $firstName= "First";
  $lastName= "Contact";
  $phoneNumber= "(415) 479 1349";
  $email= "jhon@company.com";
  $trnDateFrom= "2015-06-06 00:00:00";
  $trnDateTo= "2015-07-07 23:59:59";
  $cardType= "Visa";


  //Convert data to array to send it to the WS as JSON format
  $data = array();
  $data= array("batchIdFrom" => "$batchIdFrom", "batchIdTo" => "$batchIdTo",
                  "transactionType" => "$transactionType", "responseCode" => "$responseCode",
                  "amountFrom" => "$amountFrom", "amountTo" => "$amountTo",
                  "firstName" => "$firstName", "lastName" => "$lastName",
                  "phoneNumber" => "$phoneNumber", "email" => "$email",
                  "trnDateFrom" => "$trnDateFrom", "trnDateTo" => "$trnDateTo","cardType"=>"$cardType");
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

  //Obtain the data from the sale recently created
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
    public class Transactions
    {
        public static void findTransactions()
        {
            try
            {
                var request = (HttpWebRequest)WebRequest.Create("http://payhub.com/payhubws/api/v2/report/transactionReport");
                request.ContentType = "text/json";
                request.Method = "POST";

                var transaction = new
                {
                    batchIdFrom= "5",
                    batchIdTo= "10",
                    transactionType= "Sale",
                    responseCode= "00",
                    amountFrom= "1",
                    amountTo= "2",
                    firstName= "First",
                    lastName= "Contact",
                    phoneNumber= "(415) 479 1349",
                    email= "jhon@company.com",
                    trnDateFrom= "2015-06-06 00:00:00",
                    trnDateTo= "2015-07-07 23:59:59",
                    cardType= "Visa"
                };

                string json = JsonConvert.SerializeObject(transaction);

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
                    var response = (HttpWebResponse)request.GetResponse();
                    Console.WriteLine("\nSending 'Put' request to URL");
                    Console.WriteLine("Response Code : " + response.StatusCode);
                    if (HttpStatusCode.OK == response.StatusCode)
                    {
                        using (var reader = new StreamReader(response.GetResponseStream()))
                            result = reader.ReadToEnd();
                    }
                    else
                    {
                        using (var reader = new StreamReader(response.GetResponseStream()))
                        {
                            result = reader.ReadToEnd();
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
                                result = reader.ReadToEnd();
                            }
                        }
                    }
                }
                Console.WriteLine(return result);
            }
            catch (IOException e)
            {
                Console.WriteLine(e);
            }
        }
    }
}
```

```ruby

    require 'uri'
    require 'net/http'
    require 'json'
    
    url = URI("http://payhub.com/payhubws/api/v2/report/transactionReport")
    
    http = Net::HTTP.new(url.host, url.port)
    
    request = Net::HTTP::Post.new(url)
    request["content-type"] = 'application/json'
    request["accept"] = 'application/json'
    request["authorization"] = 'Bearer 2a5d6a73-d294-4fba-bfba-957a4948d4a3'
    request["cache-control"] = 'no-cache'

    informationToSend = {"batchIdFrom"=> "5",
                         "batchIdTo"=> "10",
                         "transactionType"=> "Sale",
                         "responseCode"=> "00",
                         "amountFrom"=> "1",
                         "amountTo"=> "2",
                         "firstName"=> "First",
                         "lastName"=> "Contact",
                         "phoneNumber"=> "(415) 479 1349",
                         "email"=> "jhon@company.com",
                         "trnDateFrom"=> "2015-06-06 00:00:00",
                         "trnDateTo"=> "2015-07-07 23:59:59",
                         "cardType"=> "Visa"
                         }                         
    
    request.body = JSON.generate(informationToSend)
    
    response = http.request(request)
    puts response.read_body
```


```json
{
  "batchIdFrom": "5",
  "batchIdTo": "10",
  "transactionType": "Sale",
  "responseCode": "00",
  "amountFrom": "1",
  "amountTo": "2",
  "firstName": "First",
  "lastName": "Contact",
  "phoneNumber": "(415) 479 1349",
  "email": "jhon@company.com",
  "trnDateFrom": "2015-06-06 00:00:00",
  "trnDateTo": "2015-07-07 23:59:59",
  "cardType": "Visa"
}
```