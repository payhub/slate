# Void & Refund Transactions
The purpose of this document is to provide you with what you need to first understand whether you need to run a void or a refund transaction, then with what you need to run either.

## Refund
```java
public class Refund {

  public static void RunRefund() {
    try {
      HttpClient client = new DefaultHttpClient();
      HttpParams params = client.getParams();
      HttpConnectionParams.setConnectionTimeout(params, 10000);

      JSONObject jsonRequestObject = new JSONObject();
      String url = "http://payhub.com/payhubws/api/v2/refund";

      JSONObject merchant = new JSONObject();
      merchant.put("organization_id", "10005");//You put your org id here
      merchant.put("terminal_id", "5");//You put your terminal id

      jsonRequestObject.put("merchant", merchant);
      jsonRequestObject.put("record_format", "CREDIT_CARD");
      jsonRequestObject.put("transaction_id", "226");

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

```json
{
  "merchant": {
    "organization_id": 10005,
    "terminal_id": 5
  },
  "record_format": "CREDIT_CARD",
  "transaction_id": "114"
}
```
Run a Refund when you need to refund the full amount of a settled sale that has not yet been refunded. The amount refunded is the same as the total amount that was originally charged in the sale. The amount is refunded to the credit or debit card that was originally charged.

### Endpoint (URL to Call):
`POST http://payhub.com/payhubws/api/refund`

### Elements
(All required)

Key | Value
--- | -----
organization_id (integer): | The merchant's organization id which did the sale you're trying to refund
terminal_id (integer): | The merchant's terminal's id where the sale was done.
record_format (string): | To specify if the sale has to be made over a credit card, debit card or cash payment. Accepted values are only: CASH_PAYMENT, CREDIT_CARD, DEBIT_CARD
transaction_id (integer): | The transaction id of the sale you want to refund.

## Void

```java
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

```json
{
  "merchant": {
    "organization_id": 10005,
    "terminal_id": 5
  },
  "transaction_id": "114"
}
```

Run a Void when you need to cancel a transaction (either a sale or a refund) that has not yet been settled and that has not already been voided. This will avoid the customer being charged any amount at all and will release the pending funds, given the issuer supports doing so.

### Endpoint (URL to Call):
`POST http://payhub.com/payhubws/api/void`

### Elements (All required)

Key | Value
--- | -----
organization_id (integer): | The merchant's organization id which did the sale you're trying to void.
terminal_id (integer): | The merchant's terminal's id where the sale was done. transaction_id (integer): | The transaction id of the sale you want to void.
