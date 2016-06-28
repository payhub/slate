# Changelog

# Release Date 2016-05-26

##Virtual Terminal (VPOS or VT)
* Added customizable Processing Modal logo
* Misc bug fixes

###recurring-bill
* Misc bug fixes

##WebService (WS)
* Misc bug fixes
* Added recurring bill update methods
* Added Transaction Totals method.
* Added Email Settings methods.

###transaction Report
* Added **SettlementStatus** to transaction result report
* Misc bug fixes

# Release Date 2016-04-27

##Virtual Terminal (VPOS or VT)
* Misc bug fixes

##WebService (WS)
* Misc bug fixes


# Release Date 2016-02-23
##WebService (WS)

###transaction Report
* Added **customerId** in the request. This allows the merchant to look for transactions that were executed by specific customers.
* Added **isCaptured** in the response. (true / false) This allows to know if an Auth Only is Captured. 
* Added **voidedBy** in the response. Shows the number of the transaction that voided the transaction searched.
* Added **refundedB** in the response. Shows (as an array) the numbers of the transactions that refunded the transaction searched.

###recurring-bill
* Misc bug fixes

###Customers > Linking Customers
* Provided documentation on how Customers are linked together

