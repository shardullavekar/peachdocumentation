# Using Go/Node JS/Java

The back end script does 3 main jobs.

* Creates Checkout

* Checks Payment Status with Peachpayments

* Capture Payment after authorised.

# Checkout

Here's how checkout is created:

``` curl
curl https://test.oppwa.com/v1/checkouts \
	-d "authentication.userId=8a8294174b7ecb28014b9699220015cc" \
	-d "authentication.password=sy6KJsT8" \
	-d "authentication.entityId=8a8294174b7ecb28014b9699220015ca" \
	-d "amount=92.00" \
	-d "currency=EUR" \
	-d "paymentType=DB" \
	-d "shopperResultUrl=my.app://custom/url" \
	-d "notificationUrl=http://www.example.com/notify"
```

!!! info "Payment Type, ShopperResult and Notification URL" 
	Payment Type could be DB or PA for direct debit or pre-authorization.
	
	ShopperResult URL should be set to your company name + ://callback. e.g. mycompanyname://callback - ensure that you give the same name to the tool when asked about company name.

	Notification URL is where we post the payment result. It's a server to server call.

# Payment Status
Here's how you can check the payment status:

``` curl
curl -G https://test.oppwa.com/v1/checkouts/{id}/payment \
	-d "authentication.userId=8a8294174b7ecb28014b9699220015cc" \
	-d "authentication.password=sy6KJsT8" \
	-d "authentication.entityId=8a8294174b7ecb28014b9699220015ca"
```

!!! info "Payment Id" 
	You will get a payment Id once the payment is completed. 

	Pass this id as shown above and check the payment status. 


# Capture Payment
After the Payment is authorized (when you initiate payment type as PA from your Android code) you need to capture it from your server.

Here's how you can capture the payment:

``` curl
curl https://test.oppwa.com/v1/payments/{id} \
	-d "authentication.userId=8a8294174e735d0c014e78cf266b1794" \
	-d "authentication.entityId=8a8294174e735d0c014e78cf26461790" \
	-d "authentication.password=qyyfHCN83e" \
	-d "amount=92.00" \
	-d "currency=EUR" \
	-d "paymentType=CP"
```

!!! info "Id" 
	First, check the payment status and get the Id from the status call. 

	Call capture payment as shown above with the Id you get from the status call. 

# Arranging Checkout, Payment Status and Capture

* Combine these 3 calls in one server script.
* This server script should accept a POST parameter called action. action could be checkout, getPaymentStatus or capture.
* For checkout, the script should accpet POST parameters amount, currency and paymentType and pass it on to checkout call.
* For payment Status, the script should accept resourcePath and pass it on to status call.
* For capturing payment, call the payment status first, get the payment id and pass it on the status check call.

Once your server script is ready, host it on your server, keep it's URL ready and proceed with the Android integration using the devsupport AI tool.

Change the following in your server script:

* Change URL from "https://test.oppwa.com/" to "https://oppwa.com/".

* Change User Id to Production User Id.

* Change Password to Production Password.

* Change Entity Id to Production Entity Id.

!!! success
	Congrats! You just completed server integration. 

	If you need help, write us an email at support@devsupport.ai - happy to help!