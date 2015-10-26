#Reserve Funding

This method you can use to board a Reserve Funding to Safyer using the Safyer Web Service.

##Request Method
###POST

##Endpoint (URL to Call): 
[https://ws.safyer.com/api/v1/reserveFunding](https://ws.safyer.com/api/v1/reserveFunding)

##Parameters
All fields are required unless indicated otherwise. All these parameters are in a JSON.

| Parameter | Type | Description |
|-----------|------|-------------|
|**mid**|Numeric|A number that identifies a Merchant.|
|**dda**|Numeric <br>(1 -30 digits)|The bank account number of the merchant.|
|**routing**|Numeric <br>(9 digits)|The routing number of the bank account of the merchant.<br>The number must match with an existing bank routing number.|
|**transferAmount**|Numeric <br>( 2 decimals)|Transfer amount.|

##Example JSON


<pre style="float: left;background-color: rgb(234, 242, 246);color: black;text-shadow: 0px 1px 2px rgba(0,0,0,0);">
    <code class="highlight shell"> 
     <span class="o" style="color:black;">{</span>
         <span class="s2"style="color:black;">"ReserveFundin"</span>: <span class="o"style="color:black;">{</span>
             <span class="s2" style="color:black;">"mid"</span>: <span class="s2" style="color:black;">"76200045009254219"</span>,
             <span class="s2" style="color:black;">"dda"</span>: <span class="s2" style="color:black;">"123456789"</span>,
             <span class="s2" style="color:black;">"routing"</span>: <span class="s2" style="color:black;">"034560015"</span>,
             <span class="s2" style="color:black;">"transferAmount"</span>: <span class="s2" style="color:black;">"128.23"</span>
         <span class="o" style="color:black;">}</span>
     <span class="o" style="color:black;">}</span>
</code>
</pre>

##Request Method

###POST

##Endpoint (URL to Call): 
[https://ws.safyer.com/api/v1/reserveFunding/{{reserveFundingId}}](https://ws.safyer.com/api/v1/reserveFunding/{{reserveFundingId}})

##Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
|reserveFundingId|Numeric|A number that identifies a Reserve Funding.|

