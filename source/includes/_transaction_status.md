# Transaction Status

<aside class="notice">
 Over the lifetime of a transaction, it will transition through several states. The table below indicates what those different states are and how to use them.
</aside>

|Status Code|Status|Description|
|-----------|------|-----------|
|N|New|Immediately after the order is submitted the status will be *New*, indicating the it has been registered as a request for a new transaction.|
|I|Pending Information|The customer will be required to provice information to confirm their identity. (We need to do this to make the lawyers happy.)  See the [identity codes](#required-info) for a description of the information a customer could be asked to provide.|
|R|Regulatory Hold| If there is a regulatory concern with the transaction it will remain in this state until we resolve the issue.|
|S|Pending SMS Authentication| This is a special type of 'Pending Information'.  Once in this state, we have sent an SMS code and are waiting for the code to be echoed back.|
|E|Expired|The window of time we provided in order to receive payment and collect the required information has elapsed. A new transaction needs to be started.|
|C|Completed|The transaction has been completed.  The customer has received their crypto currency and the vendor received payment.
|P|Pending Payment|We are waiting for a confirmation the payment has been received.|

y for a description of the pieces of information a customer may be asked to provide.|
