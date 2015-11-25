```shell

    {
      "id": "182364",
      "version": 0,
      "createdAt": "2015-07-07T08:45:15.594-07:00",
      "lastModified": "2015-07-07T08:45:15.594-07:00",
      "createdBy": "10127",
      "lastModifiedBy": "10127",
      "metaData": null,
      "merchant": {
        "id": "10127215",
        "version": 1,
        "createdAt": "2015-04-02T11:46:16.676-07:00",
        "lastModified": "2015-04-28T13:28:21.427-07:00",
        "createdBy": "10127",
        "lastModifiedBy": "10127",
        "metaData": null,
        "organization_id": 10127,
        "terminal_id": 215
      },
      "customer": {
        "id": "182364",
        "version": 0,
        "createdAt": "2015-07-07T08:45:15.584-07:00",
        "lastModified": "2015-07-07T08:45:15.584-07:00",
        "createdBy": "10127",
        "lastModifiedBy": "10127",
        "metaData": null,
        "first_name": "First",
        "last_name": "Contact",
        "email_address": "test@payhub.com",
        "phone_number": "(415) 479 1349",
        "phone_ext": "123",
        "phone_type": "M",
        "company_name": "Payhub Inc",
        "job_title": "Software Engineer",
        "web_address": "http://payhub.com",
        "customerReference": {
          "customerId": 2972,
          "customerEmail": "test@payhub.com",
          "customerPhones": []
        },
        "customerId": 182364
      },
      "card_data": {
        "id": "9999000000001853",
        "version": 248,
        "createdAt": "2015-04-02T11:46:16.457-07:00",
        "lastModified": "2015-10-22T11:42:39.589-07:00",
        "createdBy": "10127",
        "lastModifiedBy": "10127",
        "metaData": null,
        "cvv_data": null,
        "track1_data": null,
        "track2_data": null,
        "encrypted_track_data": null,
        "card_number": null,
        "card_expiry_date": "202011",
        "tokenized_card": "9999000000001853",
        "billing_address_1": "2350 Kerner Blvd",
        "billing_address_2": "On the corner",
        "billing_city": "San Rafael",
        "billing_state": "CA",
        "billing_zip": "94901",
        "cardObscured": null,
        "cardType": null
      },
      "verifyResponse": {
        "verifyId": "182364",
        "approvalCode": "VTLMC1",
        "processedDateTime": null,
        "avsResultCode": "0",
        "verificationResultCode": "",
        "responseCode": "00",
        "responseText": "APPROVAL VTLMC1",
        "cisNote": "",
        "riskStatusResponseText": "",
        "riskStatusRespondeCode": "",
        "saleDateTime": "2015-07-07 08:45:15",
        "tokenizedCard": "9999000000001853",
        "customerReference": {
          "customerId": 2972,
          "customerEmail": "test@payhub.com",
          "customerPhones": []
        },
        "cardObscured": null,
        "cardType": null
      },
      "_links": {
        "self": {
          "href": "https://api.payhub.com/api/v2/verify/182364"
        },
        "merchant": {
          "href": "https://api.payhub.com/api/v2/verify/182364/merchant"
        },
        "customer": {
          "href": "https://api.payhub.com/api/v2/verify/182364/customer"
        },
        "card_data": {
          "href": "https://api.payhub.com/api/v2/verify/182364/card_data"
        }
      }
    }
    
```