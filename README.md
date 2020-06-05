# [Team ID8: PayAway](https://github.com/)

This solution was created as part of the 2020 FIS InnovateIN48 US regionals competition.
<p>This source code repository contains:
<ul>
<li>Merchant IPhone application.  This is a native IOS application that would be installed on the Merchant's phone.
<li>Backend Business Services for the PayAway Merchant Smartphone application and Customer Payment Pages.  
The Business Services are hosted in a Microsoft Azure App Service and are exposed as a set of REST WebAPIs.
<li>Payment Page (HPP) used by the Customer.  
The HPP is built using Blazor WebAssembly and runs in a user's browser.
<li>The code for RAFT and Raft Gateway is stored in their Code Repositories.  
</ul>

---
## PayAway

#### Business Proposition
PayAway is a solution that converts remote payment transactions such as over-the-phone into secure online payments. It's value proposition rests on 2 main pillars:
- To reduce the risk associated with exchanging payment card information openly over the phone - benefits merchants and consumers
- To support a merchant's digital strategy by converting the payment portion of a phone interaction into a more secure online payment transaction, at the completion of which the consumer can be redirected to the merchants digital properties to continue the brand interaction. This opportunity for organic brand interaction would have been completely lost at the time the consumer hangs up the phone.

 

The solution can be leveraged by merchants of any size and in virtually any industry or vertical: doctor's offices can use it to accelerate time to revenue; professional services providers such as contractors can leverage it to capture security deposits for an appointment or to bill pre- or post-service; online retailers offering the increasingly popular web chat support services can leverage it to drive conversion prior to terminating contact with the customer; and many other examples. All merchants can tie the solution into their digital strategies and benefit from safer payments and increased brand interaction post-contact, simply by leveraging this innovative implementation of the payment flow.

* replace the need to take Credit Card Numbers over the phone with a very low friction e-Commerce like experience for the customer.
* remove the PCI compliance challenges merchants face 
* provide an opportunity to extend the interaction with the customer
* does not require any specialized equipment, investment or integration changes by the merchant.

#### Overview
We provide an ability for the Customer's Payment Card No to be substituted for a Token before it is made available to the Merchant.

The merchant enters the Token into their Credit Card Terminal/POS the same they normally would.

FIS/Worldpay's systems automatically convert the token back into the original PAN before sending the payment authorization request to the Card Networks.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Merchant New Order Placeholder](Images/tokenized_pan.jpg?raw=true)

#### Merchant Solution 

The Merchant solution consists of three (3) experiences

&nbsp;&nbsp;&nbsp;&nbsp;![Merchant Order Summary Placeholder](Images/merchant_order_summary.jpg?raw=true)&nbsp;&nbsp;&nbsp;&nbsp;
![Merchant New Order Placeholder](Images/merchant_new_order.jpg?raw=true)
&nbsp;&nbsp;&nbsp;&nbsp;![Merchant Enter Payment Placeholder](Images/merchant_enter_payment.jpg?raw=true)

* (Left) A panel that shows a summary of orders with tabs for different Order Statuses.  You can click on a `Card` to see additional information about that order.  You click on the `Request Payment` button to enter a new Payment Request.

* (Center) This screen is used to enter new **Payment Requests**.  The merchant could do this while on the phone with the customer or shortly after hanging up.  You only need to enter five (5) pieces of information:

    + Customer's Mobile Phone No (where will will send the SMS message)
    + Customer's Name (will personalize the SMS message)  
    + Sale Amount.
    + Merchant's Order No
    + Select the Pickup Time
        
* (Right) After the customer has enterd their payment information, the Merchant uses this screen to enter the payment details into their Credit Card Terminal or POS system.  Note: The Card No shown is a token, it **IS NOT** the same number that the customer entered! 

> @icon-info-circle Note: The Order `automatically` moves from the `Ready` screen to the `Posted` screen when the Merchant processes the payment thru their Credit Card terminal or POS.

> @icon-info-circle Note: We are not demoing but we could also support Payment Requests that are sent via Email and run in a desktop/tablet browser

> @icon-exclamation-circle Note: The Merchant Application is **NOT in PCI scope**. It DOES NOT have access to clear text PAN, it only has access to Tokenized PAN

#### Cardholder Solution

The Cardholder solution consists of three (3) experiences

&nbsp;&nbsp;&nbsp;&nbsp;![CardHolder SMS Placeholder](Images/sms.jpg?raw=true)&nbsp;&nbsp;&nbsp;&nbsp;
![CardHolder Payment Page Placeholder](Images/payment_page.jpg?raw=true)
&nbsp;&nbsp;&nbsp;&nbsp;![CardHolder Confirmation page Placeholder](Images/confirmation_page.jpg?raw=true)

* (Left) The cardholder receives an SMS message with a clickable link to a unique payment page for their order.

* (Center) Clicking on the link opens the user's browser to a branded *Payment Page* for the order where you enter only three (3) pieces of information.  The `Pay Now` button is enabled when you have entered all of the required information.

    + The *Payment Page* is dynamically branded with the Merchant's Logo to make the cardholder comfortable that this is a valid site.

* (Right) After clicking on the `Pay Now` button, the user is navigated to a read only confirmation screen with a brief toast confirmation.

    + This page contains a Merchant defined *tagline* for one last engagement with the customers.
    + Clicking on the button navigates the customer to the Merchant's website.

---
## Solution Architecture

![One Bubble Placeholder](Images/one_bubble.jpg?raw=true) &nbsp; **IPhone Smartphone Application**  
+ The IPhone application is written in [Swift](https://www.apple.com/swift/) 

+ This application interacts with the `Cloud hosted` Business Services thru a set of REST WebAPIs.

+ The following third-party libraries were used:
    + PhoneNumberKit
    + CurrecyText
    + Alamofire
    + SwiftyJSON

![AppArch](Images/two_bubble.jpg?raw=true) &nbsp; **Mobile Brower Payment Page**  
+ The Payment Page is written in [Blazor WebAssembly](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor) v3.2

+ Blazor is a .NET web framework using C#/Razor, HTML and Bootstrap that runs in the browser using WebAssembly (more information below).

+ This application interacts with the `Cloud hosted` Business Services thru a set of REST WebAPIs.
+ This application also retrieves static images (merchant logos) from an `Azure Blob Container` exposed by a Read-only Blob URL.

+ The following third-party libraries from nuget were used:
    + MatBlazor

![Three Bubble Placeholder](Images/three_bubble.jpg?raw=true) &nbsp; **Business Services**  
* Our solution built a set of WebAPIs written in C# using [.NET Core 3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1) running in ASP.NET and hosted in an `Azure App Service` that support:
    + Customer Payment Page Application
    + Merchant SmartPhone Application
    + RAFT Payment Status Event Postback

> @icon-info-circle  The WebAPIs are fully documented and testable using an associated Swagger Site.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Swagger Placeholder](Images/swagger.jpg?raw=true) 

* .NET Core is an open-source, general-purpose development framework for building cross-platform apps.

* This app uses an embedded `SQLite` database for local storage in the service. 

> @icon-info-circle  No PAN or PCI is stored in th SQLite DB.

+ The following third-party libraries from nuget were used:
    + EntityFramework Core
    + Swashbuckle
    + Twilio
    + RestSharp
    + SQLite 

![Four Bubble Placeholder](Images/four_bubble.jpg?raw=true) &nbsp; **RAFT API Gateway (RGW)**  
* Our solution modified the RGW to receive an event from RAFT (over a socket) and make an outbound WebAPI call (thru DataPower) to the PayAway Business Services hosted in Azure. 

+ RGW has a robust custom framework that supports most changes via XML config files in place of code.  An overview of the RGW changes that were required for `PayAway` is shown below:

>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Swagger Placeholder](Images/rgw.jpg?raw=true) 

+ The few code changes required are written in `JAVA` and run in a special region `on the Mainframe`, an example is shown below...

+ SendReceiveRESTMessage.java  
    * This Module handles adding HTTP headers to message payload and sending/receiving the REST message on the targeted endpoint.  Added new call type string and if clause for adding the headers needed for the PayAway web service call.

```java
private static final String CALL_TYPE_PAYAWAY = "PAYAWAY";
// existing statements removed, only this else if was added
else if (callType.toUpperCase().equals(CALL_TYPE_PAYAWAY)) {
	mainHTTP.setReqHeader("Content-Type", "application/json");
	mainHTTP.setHttpURL(new URL(urlNode.getAttributeByName(ATTR_DOC_DATA).getAttrValue()));
	mainHTTP.setRequestMethod(HTTPMessage.HTTP_REQ_POST);
	mainHTTP.setRequestBody(actionMsg.getRequestByteMsg());
	mainHTTP.setReqHeader("Authorization", "VANTIV license=\"" + config.getProperty("vts.dp") +"\"");	
}
```

![Five Bubble Placeholder](Images/five_bubble.jpg?raw=true) &nbsp; **RAFT Auth Switch (RAFT)**  
+ The fundamental requirement was to allow merchants using basic Card Terminals to continue to process clear text PAN transactions and transparently add the ability to process Tokenized transactions.

+ The challenge was how to distinguish on-the-fly between tokenized and non-tokenized payment card Nos in a payment auth request.  

+ The solution for the Hackathon was to use the Tokenization request as a `PayAway initialization transaction` and additionally store the Tokenized PAN in a special `PayAway` vault.  If the merchant is enabled for `PayAway`, Raft will 1st look in the PayAway Vault for an entry matching the PAN value received.  If it finds a match, it will then use the standard Vault to detokenize the value received back to clear text PAN.   Additionally RAFT will also send a `Payment Authorized` event to RGW.  If it does not find a matching entry in the PayAway Vault, RAFT assumes the value received is clear text PAN. 

+ The changes were written in `Mainframe Assembler`, a sample of the code is below...
```text  
*---------------------------------------------------------------------*
*        CHECK FOR PAYAWAY                                            *
*---------------------------------------------------------------------*
PAYACHK  DS    0H                                            
         STM   R0,R15,SAVELVL3         SAVE REGISTERS                  
         LR    R10,R15                 COPY ENTRY POINT                
         USING PAYACHK,R10             ADDRESS IT                      
*--------IN REAL LIFE, WE'D CHECK IF IT'S A MERCHANT ENABLED FOR       
*--------PAYWAY AND A TOKENIZATION REQUEST. IF YES. STORE THE KEY IN   
*--------THE TOKEN VAULT.                                              
         L     R3,POSPTR               LOAD OFFS TO POS AREA           
         USING TRPOSINF,R3             SET POS AREA BASE               
         CLI   TNSTAT2,TN2SPEC         Q. SPECIAL TRAN?                
         BNE   PAWY9000                N. DONE                         
         CLI   TNRQSPEC,TNTKNREQ       Q. REQUEST TOKEN FROM PAN?      
         BNE   PAWY9000                N. DONE                         
         CLC   TRMRHNO(13),=C'4445091051532' Q. TEST MID?              
         BNE   PAWY9000                N. DONE                         

```


![Six Bubble Placeholder](Images/six_bubble.jpg?raw=true) &nbsp; **Twilio SMS Service**  
* Our solution calls the [Twilo](https://www.twilio.com/) REST WebAPI to send SMS messages to customers.

&nbsp;&nbsp;&nbsp;&nbsp;![App Arch Placeholder](Images/app_arch.jpg?raw=true)

> @icon-info-circle Special thank you to the DataPower team who helped us setup a non-prod proxy for this project!

---
## Innovative Technology

> #### @icon-info-circle WebAssembly Background 
> [WebAssembly](https://webassembly.org/) (abbreviated Wasm) is an open standard that defines a portable binary-code format for executable programs.  It became a [World Wide Web Consortium recommendation](https://www.w3.org/TR/wasm-core-1/) on December 5, 2019 and, alongside HTML, CSS, and JavaScript, is the fourth language to run natively in browsers.

[Blazor]( https://blazor.net) is an open source and cross-platform web UI framework for building single-page apps (SPAs) built on `WebAssembly` that was officially release on `May 19, 2020`. 

> @icon-exclamation-circle Blazor is **NOT** a browser plugin!

Blazor apps are composed of reusable web UI components implemented using C#, HTML, and Bootstrap/CSS (instead of JavaScript). If the server code is also written in C#, you can easily share code and libraries.

Web SPAs written in Blazor look and act no different to a user (and "work" just like a standard browser app).

> @icon-info-circle Blazor has built-in support for building Progressive Web Apps (PWA) with offline capabilities.

Similar to JavaScript, Blazor WebAssembly apps run securely in the user’s browser security sandbox. 

&nbsp;&nbsp;&nbsp;&nbsp;![App Arch Placeholder](Images/blazor_webassembly.jpg?raw=true)

These apps can be deployed as completely standalone static sites without any .NET server component at all, or they can be paired with ASP.NET Core to enable full stack web development and debugging with .NET using Visual Studio, Visual Studio for Mac, and Visual Studio Code.

> @icon-info-circle In this solution, the cloud-hosted business services and the payment page running the customer's brower share the same assembly that contains definitions of Business Entities and shared logic.


---
### Mobile Browser Support for WebAssembly

* Data from https://caniuse.com/#search=webassembly

&nbsp;&nbsp;&nbsp;&nbsp;![CanIUseIt](Images/can_i_use_it.jpg?raw=true)

> @icon-exclamation-circle  WebAssembly **is** broadly supported across the Mobile Browsers used on current Smartphones.

---
## Team ID8
Team ID8 is from the Merchant Solutions division of FIS


* David Miller (Enterprise Architecture)
* Justin Nutting (Enterprise Transaction Processing)
* Marco Fernandes (Commercial Enablement & Effectiveness)
* Nick Taylor (Enterprise Transaction Processing)
* Tom Bruns (Enterprise Architecture)

