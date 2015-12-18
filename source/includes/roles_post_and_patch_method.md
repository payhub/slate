```shell
PATCH AND POST METHODS (the same json structure is used)
{
  "transactions": {
    "checked": "true",
    "transactionOptions": {
      "viewHostedShop": "true",
      "viewSubmitBatch": "true",
      "single": {
        "singleOptions": {
          "voidupto": "true",
          "sales": "true",
          "refund": "true",
          "viewTransaction": "true"
        },
        "checked": "true"
      },
      "recurringBills": {
        "recurringBillOptions": {
          "viewHostedShop": "true",
          "edit": "true",
          "delete": "true",
          "add": "true"
        },
        "checked": "true"
      },
      "currentBatches": "true",
      "txtRefundupto": "0.01",
      "allBatches": "true"
    }
  },
  "reports": {
    "reportOptions": {
      "standard": {
        "standardOptions": {
          "product": "true",
          "recurrbill": "true",
          "users": "true",
          "transaction": "true",
          "batch": "true",
          "customer": "true"
        },
        "checked": "true"
      },
      "custom": "true"
    },
    "checked": "true"
  },
  "help": {
    "helpOptions": {
      "tickets": {
        "ticketsOptions": {
          "edit": "true",
          "add": "true"
        },
        "checked": "true"
      }
    },
    "checked": "true"
  },
  "mobileVTAcces": {
    "checked": "true"
  },
  "admin": {
    "adminOptions": {
      "product": {
        "checked": "true",
        "productOptions": {
          "edit": "true",
          "delete": "true",
          "bulkUpload": "true",
          "add": "true"
        }
      },
      "users": {
        "usersOptions": {
          "deleteRole": "true",
          "manageRole": "true",
          "editRole": "true",
          "edit": "true",
          "delete": "true",
          "addRole": "true",
          "add": "true",
          "usersEditAdmin": "true"
        },
        "checked": "true"
      },
      "hostedShopCart": "false",
      "riskAndFraud": {
        "riskAndFraudOptions": {
          "riskfraudSetting": "true",
          "riskfraudFlagTrn": "true"
        },
        "checked": "true"
      },
      "webposSetUp": "true",
      "general": {
        "generalOptions": {
          "validateDev": "true",
          "thirdParyApi": "true",
          "shippingTax": "true",
          "branding": "true",
          "generalSetting": "true"
        },
        "checked": "true"
      }
    },
    "checked": "true"
  },
  "customer": {
    "customerOptions": {
      "edit": "true",
      "delete": "true",
      "view": "true,
      "add": "true"
    },
    "checked": "true"
  },
  "firstDefaultScreen": "2",
  "webPosAccess": {
    "checked": "true",
    "webPosAccessOptions": {
      "admin": {
        "adminOptions": {
          "hotKey": "true",
          "submitBatch": "2",
          "deviceList": "true",
          "generalSettings": "true",
          "tills": "true",
          "display": "true",
          "customPrompt": "true",
          "customMessage": "true"
        },
        "checked": "true"
      },
      "quitPos": "true",
      "manualPriceChange": "true"
    }
  },
  "roleName": "test4"
}
```