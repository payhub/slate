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
<pre style="float: left;background-color: rgb(234, 242, 246);color: black;text-shadow: 0px 1px 2px rgba(0,0,0,0);">
<code class="highlight shell"> 
<span class="o" style="color: black;">{</span>
     <span class="s2" style="color: black;">"ReserveFundin"</span>: <span class="o" style="color: black;">{</span>
         <span class="s2" style="color: black;">"mid"</span>: <span class="s2" style="color: black;">"7620000000000002"</span>,
         <span class="s2" style="color: black;">"dateCreateFrom"</span>: <span class="s2" style="color: black;">"03-23-2015"</span>,
         <span class="s2" style="color: black;">"dateCreateTo"</span>: <span class="s2" style="color: black;">"04-13-2015"</span>
         <span class="o" style="color: black;">}</span>
 <span class="o" style="color: black;">}</span>
</code>
</pre>
