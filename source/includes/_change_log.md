# Changelog
## Introduction

This is a summary of all the WS changes made in the last sprint.

###transaction Report
* Added **customerId** in the request. This allows the merchant to look for transactions that were executed by specific customers.
* Added **isCaptured** in the response. (true / false) This allows to know if an Auth Only is Captured. 
* Added **voidedBy** in the response. Shows the number of the transaction that voided the transaction searched.
* Added **refundedB** in the response. Shows (as an array) the numbers of the transactions that refunded the transaction searched.

###recurring-bill
* Misc bug fixes

###Customers > Linking Customers
* Provided documentation on how Customers are linked together

