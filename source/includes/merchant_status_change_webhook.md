# Merchant Status Change Webhook

This topic provides the information about the web service that you need to create to receive requests from Chargebacks.

## Request Method:
###POST

##End Point URL
Speciefied by the Sales Office in My Office Admin - MyOffice
##Request Elements (from Safyer)

<aside class='warning'>All fields are required unless indicated otherwise</aside>

|Parameter | Type | Description|
|--------- | --------- | ------------ |
|internalID|String 5 - 20 digits|The unique system identifier for the merchant issued by Safyer.|
|mid|String 5 - 20 digits|The unique Merchant Identification Number (MID) issued by the processor.|
|statusFrom|String  6 - 30 characters| <br>Old state.<br>Visa/Mastercard status.<br> Possible values:<br><ul><li>Approved</li><li>Closed</li><li>Closed with Fees</li><li>Declined</li><li>Not Submitted</li><li>Pending</li><li>Pre-Approved</li><li>Pre-Approved First Transaction</li><li>Submitted</li><li>Withdrawn</li>|
|statusTo|String 6 - 30 characters| <br>New state.<br>Visa/Mastercard status.<br> Possible values:<br><ul><li>Approved</li><li>Closed</li><li>Closed with Fees</li><li>Declined</li><li>Not Submitted</li><li>Pending</li><li>Pre-Approved</li><li>Pre-Approved First Transaction</li><li>Submitted</li><li>Withdrawn</li>|
|statusDateChange|Date Date format<br> (MM-dd-yyyy)|Date of change state.|
|userName|String 6 - 30 characters|User name that produced the status change.|

##Response Elements (to Safyer)

|Parameter | Type | Description|
|--------- | --------- | ------------ |
|code|String<br>1 digit|The response code returned in the response.<br><ul><li> 0: Success</li><li>1: Error</li></ul>|
|msg|String 2 - 5 characters|If code is 0 then msg is 'OK'. Otherwise, msg is 'ERROR'|

##Response Examples

<pre style="float: left;background-color: rgb(234, 242, 246);color: black;text-shadow: 0px 1px 2px rgba(0,0,0,0);">
<code class="highlight xml"><span class="cp" style="color: black;">&lt;?xml version="1.0" encoding="UTF-8" ?&gt;</span>
<span class="nt" style="color: black;">&lt;Response&gt;</span>
    <span class="nt" style="color: black;">&lt;code&gt;0</span><span class="nt" style="color: black;">&lt;/code&gt;</span> <span class="nt" style="color: black;">Successfully</span>
    <span class="nt" style="color: black;">&lt;msg&gt;OK</span><span class="nt" style="color: black;">&lt;/msg&gt;</span>
<span class="nt" style="color: black;">&lt;/Response&gt;</span>

<span class="cp" style="color: black;">&lt;?xml version="1.0" encoding="UTF-8" ?&gt;</span>
<span class="nt" style="color: black;">&lt;Response&gt;</span>
    <span class="nt" style="color: black;">&lt;code&gt;1</span><span class="nt" style="color: black;">&lt;/code&gt;</span> <span class="nt" style="color: black;">Error</span>
    <span class="nt" style="color: black;">&lt;msg&gt;Error</span><span class="nt" style="color: black;">&lt;/msg&gt;</span>
<span class="nt" style="color: black;">&lt;/Response&gt;</span>

</code>
</pre>
