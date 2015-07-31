# Errors

<aside class="notice">This error section is stored in a separate file in `includes/_errors.md`. Slate allows you to optionally separate out your docs into many files...just save them to the `includes` folder and add them to the top of your `index.md`'s frontmatter. Files are included in the order listed.</aside>

The Kittn API uses the following error codes:


Error Code | Meaning
---------- | -------
00 | VALID_OPERATION
02 | CARD_NUMBER_MISSING_CODE
1009 | INVALID_REFERENCE_NUMBER
4006 | INVALID_MERCHANT
4007 | INVALID_TERMINAL
4008 | INVALID_USER_NAME
4009 | INVALID_USER_PASSWORD
4010 | INVALID_RECORD_FORMAT
4011 | INACTIVE_TERMINAL
4012 | INVALID_AUTHENTICATION
4013 | INVALID_TRANSACTION_CD
4014 | INVALID_OFFLINE_APPROVAL_CD
4015 | INVALID_CARDHOLDER_ID_CODE
4016 | INVALID_CARD_HOLDER_ID_DATA
4017 | INVALID_ACCOUNT_DATA_SOURCE
4018 | INVALID_CUSTOMER_DATA_FIELD
4019 | INVALID_CVV_CODE
4020 | INVALID_CVV_DATA
4021 | INVALID_TRANSACTION_AMOUNT
4022 | INVALID_CARD_NUMBER
4023 | INVALID_BATCH_ID
4024 | INVALID_TRANSACTION_ID
4025 | INVALID_CARD_EXPIRY_DATE
4026 | INVALID_AVS_DATA_FLAG
4027 | INVALID_CUSTOMER_ID
4028 | INVALID_CUSTOMER_WEB
4029 | INVALID_CUSTOMER_EMAIL_ID
4030 | INVALID_CUSTOMER_BILLING_ADD_ZIP
4031 | INVALID_CUSTOMER_SHIPPING_ADD_ZIP
4032 | INACTIVE_MERCHANT
4033 | INVALID_TERMINAL_ORIGIN
4034 | INVALID_CARD_DATA_FOR_DEBIT
4035 | INVALID_TRANSACTION_CODE_FOR_DEBIT
4036 | INVALID_CUSTOMER_ADDRESS
4037 | INVALID_CUSTOMER_COMPANY_NAME
4038 | INVALID_CUSTOMER_DATA
4039 | INVALID_TRANSACTION_NOTE
4040 | CARD_NOT_SUPPORT_CODE
4041 | CVV_REQUIRED_CODE
4042 | AVS_REQUIRED_CODE
4043 | CARD_REQUIRED_CODE
4044 | EXPIRY_REQUIRED_CODE
4045 | TRACK_DATA_REQUIRED_CODE
4046 | CARD_TOKEN_GENRATION_FAILED_CODE
4047 | PAYMENT_TYPE_REQUIRED_CODE
4048 | TRANSACTION_TYPE_REQUIRED_CODE
4049 | TRANSACTION_ID_REQUIRED_CODE
4050 | BATCH_ID_REQUIRED_CODE
4051 | TERMINAL_ID_REQUIRED_CODE
4052 | ORGANIZATION_REQUIRED_CODE
4053 | BATCH_TRANSACTION_NOT_FOUND_CODE
4054 | ALREADY_SETTLED_BATCH_CODE
4055 | DUPLICATE_BATCH_CODE
4056 | BATCH_SETTLED_SUCCESSFULLY_CODE
4057 | BATCH_SETTLEMENT_FAILD_CODE
4058 | BATCH_REJECTED_CODE
4059 | NETWORK_UNAVAILABLE_CODE
4060 | UNABLE_TO_BUILT_REQUEST_CODE
4061 | RECORD_NOT_FOUND_CODE
4062 | RECURRING_SAVED_CODE
4063 | RECURRING_SAVING_FAILED_CODE
4064 | RECURRING_UPDATION_CODE
4065 | RECURRING_UPDATION_FAILED_CODE
4066 | RECURRING_STATUS_CHANGED_SUCCESSFULLY
4067 | RECURRING_STATUS_CHANGING_FAILED
4068 | INVALID_TRANSACTION_ID_CODE
4069 | TRANSACTION_ALREADY_VOIDED_CODE
4070 | CARD_TYPE_REQUIRED_CODE
4071 | INVALID_BATCH_NO_CODE
4072 | TRANSACTION_ALREADY_REFUNDED_CODE
4073 | UNABLE_TO_VOID_CODE
4074 | UNABLE_TO_REFUND_CODE
4075 | UNABLE_TO_CAPTURE_CODE
4076 | CAPTURED_TRANSACTION_SENT_CODE
4077 | CAPTURED_TRANSACTION_FAILED_CODE
4078 | INVALID_INVOICE_NUMBER
4080 | INVALID_BILL_TYPE_CODE
4081 | INVALID_BILL_GENERATION_SPAN_CODE
4082 | INVALID_END_DATE_TYPE_CODE
4083 | INVALID_END_BILL_COUNT_CODE
4084 | INVALID_END_BILL_DATE_CODE
4085 | INVALID_WEEK_DAYS_CODE
4086 | INVALID_MONTHLY_TYPE_CODE
4087 | INVALID_MONTHLY_WEEK_DAYS_CODE
4088 | INVALID_MONTHLY_DAYS_POSSION_CODE
4089 | INVALID_MONTHLY_DAYS_CODE
4090 | INVALID_START_DATE_CODE
4091 | INVALID_SPECIFIC_DATES_CODE
4092 | SPECIFIC_SAME_DATES_CODE
4093 | INVALID_RECURRING_DATA_CODE
4094 | END_BILL_DATE_BEFORE_START_CODE
4095 | CARD_HOLDER_DATA_REQUIRED_CODE
4096 | CARD_HOLDER_CODE_REQUIRED_CODE
4097 | INVALID_STATUS_CODE
4098 | UNABLE_TO_CHANGE_STATUS
4099 | CARD_VALIDATION_FAILED_CODE
4100 | RECURRING_CIS_FAILED_CODE
4101 | UNABLE_BUILT_RECURRING_FILTER__CODE
4103 | UNABLE_TO_BUILT_NEXT_BILL_CODE
4501 | INVALID_PHONE_TYPE
4502 | INVALID_PHONE_NUMBER
4503 | INVALID_ZIP_CODE
4504 | INVALID_STATE_CODE
4505 | INVALID_INTERVAL_TYPE
4506 | INVALID_END_DATE
4507 | INVALID_BILL_WEEKLY_DAYS
4508 | INVALID_YEAR_MONTH_DAYS
4509 | INVALID_YEAR_BILL_ON_MONTH_NO
4510 | INVALID_MONTHLY_BILL_TYPE
4511 | INVALID_MDAYS_POSITION
4512 | INVALID_MDAYS
4513 | INVALID_MDAY_EACH_POSITIONS
4514 | INCONSISTENT_SCHEDULE_FIELDS
4515 | ACCOUNT_IS_NOT_ADMINISTRATOR
4516 | INCONSISTENT_CARD_DATA_FIELDS
4517 | WRONG_OAUTH_TOKEN_FOR_MERCHANT
4518 | NO_MERCHANT_ACCOUNT_FOR_OUTH_TOKEN_FOR_MERCHANT
4519 | NO_SETUP_ACCOUNT_FOR_OUTH_TOKEN_FOR_MERCHANT
4520 | NO_VT_CLIENT_ACCOUNT_FOR_OUTH_TOKEN_FOR_MERCHANT
4521 | INVALID_CURRENCY_CODE
4522 | INVALID_RECURRING_BILL_STATUS_CHANGE_CODE
4523 | INCONSISTENT_CURRENCIES
4524 | MISSING_PROPERTY_CODE
4525 | INCONSISTENT_CUSTOMER_DATA_CODE
4526 | DUPLICATE_DEVELOPER_CODE
9989 | METHOD_REQUEST_BODY_VALIDATION_ERROR_CODE
9990 | HTTP_NOT_READABLE_EXCEPTION_CODE
9991 | JSONPARSE_EXCEPTION_CODE
9992 | MAPPING_EXCEPTION_CODE
9993 | MEDIA_TYPE_NOT_SUPPORTED_CODE
9994 | METHOD_NOT_SUPPORTED_CODE
9995 | PAGE_NOT_FOUND_ERROR_CODE
9996 | ACCESS_DENIED_ERROR_CODE
9997 | JSON_SYNTAX_ERROR_CODE
9998 | NOT_FOUND_ERROR_CODE
9999 | INTERNAL_SERVER_ERROR_CODE