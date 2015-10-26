# Response Codes
|Response Code|Response Text|Description|
|------|-----------|--------------
|00|Approval|Successful - Approved and completed|
|01|Call|Failed - Refer to issuer|
|02|Call|Failed - Refer to issuer-Special condition|
|03|Term ID Error No Merchant|Failed - Invalid Merchant ID|
|04|Hold-call or Pick Up Card|Failed - Pick up card (no fraud)|
|05|Decline|Failed - Do not honor|
|06|Error XXXX|Failed - General error|
|06*|(Check Service Custom Text)|Failed - Error response text from check service|
|07|Hold-call or Pick Up Card|Failed - Pick up card, special condition (fraud account)|
|08|Approval|Successful - Honor MasterCard with ID|
|10|Partial Approval|Failed - PayHub does not support partial approvals.|
|11|Approval|Successful - VIP approval|
|12|Invalid Trans|Failed - Invalid transaction|
|13|Amount Error|Failed - Invalid amount|
|14|Card No. Error|Failed - Invalid card number|
|15|No such Issuer|Failed - No such issuer|
|19|RE Enter|Failed - Re-enter transaction|
|21|No Action Taken|Failed - Unable to back out transaction|
|28|No Reply|Failed - File is temporarily unavailable|
|34|Transaction Cancelled|Failed - MasterCard use only, Transaction Cancelled; Fraud Concern (Used in reversal requests only)|
|39|No Credit Acct|Failed - No credit account|
|41|Hold-call or Pick Up Card|Failed - Lost card, pick up (fraud account)|
|43|Hold-call or Pick Up Card|Failed - Stolen card, pick up (fraud account)|
|51|Decline|Failed - Insufficient funds|
|52|No Check Account|Failed - No checking account|
|53|No Save Account|Failed - No savings account|
|54|Expired Card|Failed - Expired card|
|55|Wrong PIN|Failed - Incorrect PIN|
|57|Serv not allowed|Failed - Transaction not permitted-Card|
|58|Serv not allowed|Failed - Transaction not permitted-Terminal|
|59|Serv not allowed|Failed - Transaction not permitted-Merchant|
|61|Declined|Failed - Exceeds withdrawal limit|
|62|Declined|Failed - Invalid service code, restricted|
|63|Sec Violation|Failed - Security violation|
|65|Declined|Failed - Activity limit exceeded|
|75|PIN Exceeded|Failed - PIN tried exceeded|
|76|Unsolicated Reversal|Failed - Unable to locate, no match|
|77|No Action Taken|Failed - Inconsistent data, reversed, or repeat|
|78|No Account|Failed - No account|
|79|Already Reversed|Failed - Already reversed at switch|
|80|Date Error|Failed - Invalid date|
|81|Encryption Error|Failed - Cryptographic error|
|82|Incorrect CVV|Failed - CVV data is not correct|
|83|Cannot Verify PIN|Failed - Cannot verify PIN|
|85|Card OK|Successful - No reason to decline|
|86|Cannot Verify PIN|Failed - Cannot verify PIN|
|91|No Reply|Failed - Issuer or switch is unavailable|
|92|Invalid Routing|Failed - Destination not found|
|93|Decline|Failed - Violation, cannot complete|
|94|Duplicate Trans|Failed - Unable to locate, no match|
|96|System Error|Failed - System malfunction|