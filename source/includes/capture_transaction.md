# Capture Transaction

This topic provides information about the **Capture** transaction.

Use the Capture transaction as the final step to collect the funds that you had asked for authorization through the AuthOnly transaction. You need to provide the transaction ID that you receive when you run the **AuthOnly** transaction.

In case the order fulfilment process, such as *shipping goods some days after they were ordered*, is delayed you might want to first run the AuthOnly transaction and then capture the respective funds. You might also need to change the amount that you got an authorization for. In that case, you need to provide a new bill section, including base amount and, if required, shipping amount, tax amount, invoice number, PO number, and note.

If the Capture transaction executes successfully, you will receive a 201 Created code. This code will include the transaction id. This transaction ID should be the same ID that you provided, which means, the same transaction id of the AuthOnly transaction that you just captured.

After capturing the amount for the AuthOnly transaction, the transaction will become a sale. The money will be taken from the card holder’s account and credited to your account during the settlement of the sale.

### Request Method

### Endpoint (URL to Call)

`http://payhub.com/payhubws/api/v2/capture`

## Elements

### merchant (required)

Key | Type | Value
--- | ---- | -----
organization_id | integer | The organization id of the merchant who did the AuthOnly that you are trying to capture.
terminal_id | integer | The terminal's id of the merchant where the AuthOnly was done.

### transaction_id (integer, required)

The transaction id of the AuthOnly that you want to capture.

### bill (optional)

Key | Type | Value
--- | ---- | -----
base_amount | TransactionAmount | The base amount of the recurring bill. The total amount charged will be the sum of this amount and the 'shipping_amount', 'tax_amount' if any.
shipping_amount | TransactionAmount, optional | The shipping amount for the transaction. This amount will be included in the total amount charged.
tax_amount | TransactionAmount, optional | The tax amount for the transaction. This amount will be included in the total amount charged.
invoice_number | string, optional | The invoice number of the transaction.
po_number | string, optional | The purchase order number of the transaction.
note | string, optional | A free format note for the transaction. The note will be read by the Merchant.

### Code without new amount

```json
{
"merchant" : {
 "organization_id" : 10005,
 "terminal_id" : 5

 },

 "transaction_id":"114"
}
```
## Code with new amount
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

## Result

* 201 code (created)
* The Id of the Capture in the Location header. This ID should be the same that you provided. <br>Sample header in the response:Location: http://payhub.com/payhubws/api/v2/authOnly/234
