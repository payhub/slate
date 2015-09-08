# Reporting Methods

## AVAILABLE RESOURCES

The Safyer Web Service provides the following resources to report on merchant activity.

Resource | Type
--- | ---
batchReport | POST
batch | GET
transaction | GET
depositReport	| POST
deposit	| GET
statementReport | POST
chargebackReport | POST

## batchReport

Retrieves information from all transactions of a Merchant for a specific date range. The date range cannot be greater than 30 days. The response will include the IdBatch element, which can be referenced using the GET batch method.

#### Request Method:
POST

#### Endpoint (URL to Call):
`https://ws.safyer.com/api/v1/batchReport`

#### Parameters
All these parameters are in a JSON:

Parameter | Type | Description
--- | --- | ---
mid	| String |
dateCreateFrom | Date, Optional |	Format mm-dd-yyyy
dateCreateTo | Date, Optional |	Format mm-dd-yyyy

#### Example JSON
```
{
    "BatchReport": {
        "mid": "7620000000000002",
        "dateCreateFrom": "03-23-2015",
        "dateCreateTo": "04-13-2015"
    }
}
```

## batch

Retrieves information from all transactions for a Batch ID (for a specific day and Merchant). The response will include the transactionId element which can be referenced using the GET transaction method.

#### Request Method:
GET

#### Endpoint (URL to Call):
`https://ws.safyer.com/api/v1/batch/{{batchId}}`

#### Parameters

Parameter | Type | Description
--- | --- | ---
iBatch | Numeric | A number that identifies a single batch.<br>**Note:** The idBatch element can be found using the POST batchReport method.

## transaction

Retrieves all information of a specific transaction.

#### Request Method:
GET

#### Endpoint (URL to Call):
`https://ws.safyer.com/api/v1/transaction/{{transactionId}}`

#### Parameters

Parameter | Type | Description
--- | --- | ---
idTransaction | Numeric | A number that identifies a single transaction.<br>**Note:** The idTransaction element can be found using the GET batch method.

## depositReport

Retrieves information from all deposits of a Merchant, between a range of dates. The response will include the idDeposit element, which can be referenced using the GET deposit method.

#### Request Method:
POST

#### Endpoint (URL to Call):
`https://ws.safyer.com/api/v1/depositReport`

#### Parameters

Parameter | Type | Description
--- | --- | ---
mid | string |
dateFrom | date, optional | Format mm-dd-yyyy.
dateTo | date, optional | Format mm-dd-yyyy.

#### Example JSON
```
{
    "Deposit": {
        "mid": "7620000000000002",
        "dateFrom": "03-23-2015",
        "dateTo": "04-13-2015"
    }
}
```
**Notes:**

* If the date range is greater than 31 days or if you omit the dateTo element, the results returned will be 31 days from the dateFrom element.
* If you omit the dateFrom element, the web service will assume a dateFrom of 31 days prior to the dateFrom value.
* If you do not indicate the date range, the web service will return deposits from the current date.

## deposit

Retrieves all information of a specific deposit.

#### Request Method:
GET

#### Endpoint (URL to Call):
`https://ws.safyer.com/api/v1/deposit/{{depositId}}`

#### Parameters

Parameter | Type | Description
--- | --- | ---
iDeposit | numeric | A number that identifies a single transaction.<br>**Note:** The idDeposit element can be found using the POST depositReport method.

## statementReport

Returns the Merchant Statement XML for a specific month and year. This XML is received from the merchant's processor and relayed as it is.

#### Request Method:
POST

#### Endpoint (URL to Call):
`https://ws.safyer.com/api/v1/statementReport`

#### Parameters

Parameter | Type | Description
--- | --- | ---
mid | string | The number of a merchant.
stmtMonth | numeric | A number between 1 and 12.
stmtYear | numeric | A number between 0000 and 9999.

#### Example JSON
```
{
   "Statement": {
        "mid": "7620000000000002",
        "stmtMonth": "2",
        "stmtYearSt": "2015"
    }
}
```

## chargebackReport

Returns information, the chargebacks for the specified merchant.

#### Request Method:
POST

#### Endpoint (URL to Call):
`https://ws.safyer.com/api/v1/chargeback`

#### Parameters

Parameter | Type | Description
--- | --- | ---
mid | string | The number of a merchant.
transactionDateFrom | date, optional | Format mm-dd-yyyy.
transactionDateTo | date, optional | Format mm-dd-yyyy.
receivedDateFrom | date, optional | Format mm-dd-yyyy.
receivedDateTo | date, optional | Format mm-dd-yyyy.
cardNumlast4 | numeric, optional | A number between 0000 and 9999.
authorizationCode | string, optional | A string of 6 characters.

#### Example JSON
```
{
    "Chargeback": {
        "mid": "7620000000000002",
        "transactionDateFrom": "",
        "transactionDateTo": "",
        "receivedDateFrom": "",
        "receivedDateTo": "",
        "cardNumLast4": "",
        "authorizationCode": ""
    }
}
```
**Note:** The processor does not provide a unique number that can be used to tie the chargeback to the original transaction.
