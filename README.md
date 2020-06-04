# [Team ID8: PayAway](https://github.com/)

This solution was created as part of the 2020 FIS InnovateIN48 US regionals competition.
<p>This source code repository contains:
<ul>
<li>Merchant IPhone application.  This is a native IOS application.
<li>Backend Business Services for the PayAway Merchant Smartphone application.  
The Business Services are hosted in a Microsoft Azure App Service and are exposed as a set of REST WebAPIs.
<li>Payment Page (HPP) used by the Customer.  
The HPP is built using Blazor WebAssembly and runs in a user's browser.
</ul>

---
## PayAway Solution Introduction

PayAway is ..... Need Marco words here


#### Merchant Solution 

The Merchant solution consists of three (3) experiences

&nbsp;&nbsp;&nbsp;&nbsp;![Merchant Order Summary Placeholder](images/merchant_order_summary.jpg?raw=true)&nbsp;&nbsp;&nbsp;&nbsp;
![Merchant New Order Placeholder](images/merchant_new_order.jpg?raw=true)
&nbsp;&nbsp;&nbsp;&nbsp;![Merchant Enter Payment Placeholder](images/merchant_enter_payment.jpg?raw=true)

* (Left) A panel that shows a summary of orders with tabs for different Order Statuses.  You can click on a `Card` to see additional information about that order.  You click on the `Request Payment` button to enter a new Payment Request.

* (Center) This screen is used to enter new **Payment Requests**.  The merchant could do this while on the phone with the customer or shortly after hanging up.  You only need to enter five (5) pieces of information:

    + Customer's Mobile Phone No (where will will send the SMS message)
    + Customer's Name (will personalize the SMS message)  
    + Sale Amount.
    + Merchant's Order No
    + Select the Pickup Time
        
* (Right) After the customer has enterd their payment information, the Merchant uses this screen to enter the payment details into their Credit Card Terminal or POS system.

> @icon-info-circle Note: The Order automatically moves from the `Ready` screen to the `Posted` screen when the Merchant processes the payment thru their Credit Card terminal or POS.

> @icon-info-circle Note: We are not demoing but we could also support Payment Requests that are sent via Email and run in a desktop/tablet browser

> @icon-exclamation-circle Note: The Merchant Application is **NOT in PCI scope**. It DOES NOT have access to clear text PAN, it only has access to Tokenized PAN

#### Cardholder Solution

The Cardholder solution consists of three (3) experiences

&nbsp;&nbsp;&nbsp;&nbsp;![CardHolder SMS Placeholder](images/sms.jpg?raw=true)&nbsp;&nbsp;&nbsp;&nbsp;
![CardHolder Payment Page Placeholder](images/payment_page.jpg?raw=true)
&nbsp;&nbsp;&nbsp;&nbsp;![CardHolder Confirmation page Placeholder](images/confirmation_page.jpg?raw=true)

* (Left) The cardholder receives a SMS message with a clickable link to a unique payment page for their order.

* (Center) Clicking on the link opens the user's browser to a branded *Payment Page* for the order where you enter only three (3) pieces of information.  The `Pay Now` button is enabled when you have entered all of the required information.

    + The *Payment Page* is dynamically branded with the Merchant's Logo to make the cardholder comfortable that this is a valid site.

* (Right) After clicking on the `Pay Now` button, the user is navigated to a read only confirmation screen with a brief toast confirmation.

    + This page contains a Merchant defined *tagline* for one last engagement with the customers.
    + Clicking on the button navigates the customer to the Merchant's website.

---
## Solution Architecture

![One Bubble Placeholder](images/one_bubble.jpg?raw=true) &nbsp; **IPhone Smartphone Application**  
+ The IPhone application is written in [Swift](https://www.apple.com/swift/) ... (Dave to add text)

+ This application interacts with the `Cloud hosted` Business Services thru a set of REST WebAPIs.

![AppArch](images/two_bubble.jpg?raw=true) &nbsp; **Mobile Brower Payment Page**  
+ The Payment Page is written in [Blazor WebAssembly](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor) v3.2

+ Blazor is a .NET web framework using C#/Razor, HTML and Bootstrap that runs in the browser with WebAssembly (more information below).

+ This application interacts with the `Cloud hosted` Business Services thru a set of REST WebAPIs.
+ This application also retrieves static images (merchant logos) from an `Azure Blob Container` exposed by a Read-only Blob URL.

![Three Bubble Placeholder](images/three_bubble.jpg?raw=true) &nbsp; **Business Services**  
* Our solution built a set of WebAPIs written in C# using [.NET Core 3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1) running in ASP.NET and hosted in an `Azure App Service` that support:
    + Customer Payment Page Application
    + Merchant SmartPhone Application
    + RAFT Payment Status Event Postback

* .NET Core is an open-source, general-purpose development framework for building cross-platform apps.

> @icon-info-circle  The WebAPIs are fully documented and testable using an associated Swagger Site.

![Four Bubble Placeholder](images/four_bubble.jpg?raw=true) &nbsp; **RAFT API Gateway (RGW)**  
* Our solution modified the RGW to receive an event from RAFT (over a socket) and make an outbound WebAPI call (thru DataPower) to the PayAway Business Services hosted in Azure. 

+ These changes were written in `JAVA` that runs in a special region on the Mainframe.

![Five Bubble Placeholder](images/five_bubble.jpg?raw=true) &nbsp; **RAFT Auth Switch (RAFT)**  
* Our solution modified the incoming Payment Authorization logic to send `Payment Authorized` events to RGW for requests from merchants configured for PayAway when they use a PayAway token.

+ These changes were written in `Mainframe Assembly`.

![Six Bubble Placeholder](images/six_bubble.jpg?raw=true) &nbsp; **Twilio SMS Service**  
* Our solution calls the [Twilo](https://www.twilio.com/) REST WebAPI to send SMS messages to customers.

&nbsp;&nbsp;&nbsp;&nbsp;![App Arch Placeholder](images/app_arch.jpg?raw=true)

---
## Innovative Technology

> #### @icon-info-circle WebAssembly Background 
> [WebAssembly](https://webassembly.org/) (abbreviated Wasm) is an open standard that defines a portable binary-code format for executable programs.  It became a [World Wide Web Consortium recommendation](https://www.w3.org/TR/wasm-core-1/) on December 5, 2019 and, alongside HTML, CSS, and JavaScript, is the fourth language to run natively in browsers.

[Blazor]( https://blazor.net) is an open source and cross-platform web UI framework for building single-page apps (SPAs) built on `WebAssembly` that was officially release on `May 19, 2020`. 

> @icon-exclamation-circle Blazor is not a browser plugin!

Blazor apps are composed of reusable web UI components implemented using C#, HTML, and Bootstrap/CSS instead of JavaScript. If the server code is also written in C#, you to share code and libraries.

Web SPAs written in Blazor look and feel no different to a user (and "work" just like a standard browser app).

> @icon-info-circle Blazor has built-in support for building Progressive Web Apps (PWA) with offline capabilities.

Similar to JavaScript, Blazor WebAssembly apps run securely on the user’s browser’s security sandbox. 

&nbsp;&nbsp;&nbsp;&nbsp;![App Arch Placeholder](images/blazor_webassembly.jpg?raw=true)

These apps can be deployed as completely standalone static sites without any .NET server component at all, or they can be paired with ASP.NET Core to enable full stack web development and debugging with .NET using Visual Studio, Visual Studio for Mac, and Visual Studio Code.

> @icon-info-circle In this solution, the cloud-hosted business services and the payment page running the customer's brower share the same assembly that contains definitions of Business Entities and shared logic.



---
### Mobile Browser Support for WebAssembly

* Data from https://caniuse.com/#search=webassembly

&nbsp;&nbsp;&nbsp;&nbsp;![CanIUseIt](images/can_i_use_it.jpg?raw=true)

> @icon-exclamation-circle  WebAssembly **is** broadly supported across the Mobile Browsers used on current Smartphones.

---
## Team ID8
Team ID8 is from the Merchant Solutions division of FIS


* David Miller (Enterprise Architecture)
* Justin Nutting (Enterprise Transaction Processing)
* Marco Fernandes (Commercial Enablement & Effectiveness)
* Nick Taylor (Enterprise Transaction Processing)
* Tom Bruns (Enterprise Architecture)

