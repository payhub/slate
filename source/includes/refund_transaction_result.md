```shell
{
        "version": 0,
        "createdAt": "2015-05-15T13:16:27.466-07:00",
        "lastModified": "2015-05-15T13:16:27.466-07:00",
        "createdBy": "10127",
        "lastModifiedBy": "10127",
        "metaData": null,
        "transaction_id": 181293,
        "record_format": "CREDIT_CARD",
        "lastRefundResponse": {
            "saleTransactionId": "43063",//it will be empty if the refund is not associated to one transaction.
            "refundTransactionId": "43064",
            "token": null,
            "approvalCode": "      ",
            "processedDateTime": null,
            "avsResultCode": "0",
            "verificationResultCode": "",
            "batchId": "133",
            "responseCode": "00",
            "responseText": "OFFLINE APPROVAL",
            "cisNote": "",
            "riskStatusResponseText": "",
            "riskStatusResponseCode": "",
            "refundDateTime": "2016-01-20 17:46:36",
            "customerReference": {
              "customerId": 0,
              "customerEmail": "",
              "customerPhones": []
            },
            "billingReferences": {
              "tokenizedCard": "9999000000001998",
              "customerId": 0,
              "customerCardId": 486,
              "customerBillId": 1,
              "cardObscured": "XXXXXXXXXXXX4507",
              "cardType": "MasterCard"
            }
          },
        "settlementStatus": "Not settled",
        
        "merchantOrganizationId": 10127,
        "_links": {
          "self": {
            "href": "https://api.payhub.com/api/v2/refund/181332"
          },
          "merchant": {
            "href": "https://api.payhub.com/api/v2/refund/181332/merchant"
          }
        }
      }
```
