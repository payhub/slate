#Reserve Funding

This method you can use to board a Reserve Funding to Safyer using the Safyer Web Service.

##Request Method
###POST

##Endpoint (URL to Call): 
[https://ws.safyer.com/api/v1/reserveFunding](https://ws.safyer.com/api/v1/reserveFunding)

##Parameters
All fields are required unless indicated otherwise. All these parameters are in a JSON.

| Parameter | Type | Description |
|-----------|------|-------------|
|**mid**|Numeric|A number that identifies a Merchant.|
|**dda**|Numeric <br>(1 -30 digits)|The bank account number of the merchant.|
|**routing**|Numeric <br>(9 digits)|The routing number of the bank account of the merchant.<br>The number must match with an existing bank routing number.|
|**transferAmount**|Numeric <br>( 2 decimals)|Transfer amount.|

##Example JSON
```shell
    {
     "ReserveFunding": {
         "mid": "76200045009254219",
         "dda": "123456789",
         "routing": "034560015",
         "transferAmount": "128.23"
        }
    }
```

##Request Method

### GET

##Endpoint (URL to Call): 
[https://ws.safyer.com/api/v1/reserveFunding/ {{reserveFundingId}}](https://ws.safyer.com/api/v1/reserveFunding/{{reserveFundingId}})

##Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
|reserveFundingId|Numeric|A number that identifies a Reserve Funding.|


