```shell
PATCH METHOD
{
  "transaction_volume_settings": {
    "checked": "false",
    "refund_trn_amount_below": {
      "value": "10",
      "option": "1"
    },
    "hours_trn_number_more_than": {
      "value": "10",
      "option": "1"
    },
    "days_trn_amount_more_than": {
      "value": "10",
      "option": "1"
    },
    "sale_trn_amount_below": {
      "value": "10",
      "option": "1"
    },
    "refund_trn_amount_above": {
      "value": "10",
      "option": "1"
    },
    "days_trn_number_more_than": {
      "value": "10",
      "option": "1"
    },    
    "sale_trn_amount_above": {
      "value": "10",
      "option": "1"
    }
  },
  "card_filtering": {
    "checked": "true"
  },
  "credit_card_security_codes": {
    "cvv_mismatch": {
      "value": "10",
      "option": "1"
    },
    "checked": "true"
  },
  "email": {
    "checked": "true",
    "email_address": "agustinbreit@gmail.com"
  },
  "address_verification_system": {
    "checked": "true",
    "avs_mismatch_street_and_zip_code": {
      "value": "10",
      "option": "1"
    },
    "avs_mismatch_street": {
      "value": "10",
      "option": "1"
    },
    "avs_mismatch_zip_code": {
      "value": "10",
      "option": "1"
    }    
  }
}
```