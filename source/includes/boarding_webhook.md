# Boarding Webhook

## Introduction

This topic provides the information about the web service that you need to create to receive requests from the Boarding WebHook. Using this WebHook, you can make your system aware of the PayHub login credentials whenever a merchant is boarded through a Frictionless Application.

Refer the information in this topic as the Request and Response format specification for the Boarding Webhook, which runs and queries the system for new merchants that have been boarded to PayHub.

### Request Method

`POST`

### End Point URL

Specified by the Sales Office in the Manage Frictionless Links section.

## Request Elements (from Safyer)

All fields are required unless indicated otherwise

Parameter | Type | Description
--- | --- | ---
internalID	 | string, 5 - 20 digits | The unique system identifier for the merchant issued by Safyer.
mid | string, 5 - 20 digits | The unique Merchant Identification Number (MID) issued by the processor.
detail | string, optional, (0 - 100 characters) | A string of data sent by the Sales Office in the query string.
payHub_OrgID | string, 1 - 11 numbers | The unique system identifier for the merchant issued by PayHub.
payHub_UserID | string, 8 - 20 characters | The username generated for the default PayHub user.
payHub_Password | string, 5 - 20 characters | The password generated for the default PayHub user.
payHub_AuthToken | string, optional, 0 - 100 characters | The OAuth Token used to authenticate the PayHub v2 API.
termID | string, 4 - 20 characters | The terminal number associated with the PayHub account.

## Response Elements (to Safyer)

Parameter | Type | Description
--- | --- | ---
code | string, 1 digit | The response code returned in the response.<br><ul><li>0: ‘Success’</li><li>1: ‘Error’</li></ul>
msg	| string, 2 - 5 characters | If code is 0 then msg is ‘OK’. Otherwise, msg is ‘ERROR’.
<pre style="float: left;background-color: rgb(234, 242, 246);color: black;text-shadow: 0px 1px 2px rgba(0,0,0,0);">
<code class="highlight xml"><span class="cp" style="color: black;">&lt;?xml version="1.0" encoding="UTF-8" ?&gt;</span>
<span class="nt" style="color: black;">&lt;Response&gt;</span>
    <span class="nt" style="color: black;">&lt;code&gt;0</span><span class="nt" style="color: black;">&lt;/code&gt;</span> <span class="nt" style="color: black;">Successfully</span>
    <span class="nt" style="color: black;">&lt;msg&gt;OK</span><span class="nt" style="color: black;">&lt;/msg&gt;</span>
<span class="nt" style="color: black;">&lt;/Response&gt;</span>
</code>
</pre>
