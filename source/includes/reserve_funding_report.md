#Reserve Funding Report
Retrieves information from a Reserve Funding for a Merchant ID or/and a specific Date Range.

##Request Method

###POST

##Endpoint (URL to Call): 
[https://ws.safyer.com/api/v1/reserveFundingReport](https://ws.safyer.com/api/v1/reserveFundingReport)

##Parameters
All these parameters are in a JSON:

| Parameter | Type | Description |
|-----------|------|-------------|
|mid|String| A number that identifies a Merchant.|
|dateCreateFrom|Date, Optional|Format mm-dd-yyyy|
|dateCreateTo|Date, Optional|Format mm-dd-yyyy|

##Example JSON
```shell
{
     "ReserveFunding": {
         "mid": "7620000000000002",
         "dateCreateFrom": "03-23-2015",
         "dateCreateTo": "04-13-2015"
         }
 }
```
