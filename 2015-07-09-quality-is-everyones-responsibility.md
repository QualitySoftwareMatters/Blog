Have you ever had this experience before?

![chase-atm-cash](https://cloud.githubusercontent.com/assets/177508/8562746/7196f316-24fb-11e5-9b6b-bdba0a194714.jpg)

I drove up to a Chase ATM to transfer money from my Liquid account (re-loadable debit card we use for budgeting purposes) to our main checking account.  (I know - it would be nice if I could just transfer this stuff online.  Unfortunately, the Liquid account does not allow direct online transfers.)  

First, I withdrew the $180 in the usual denomination of $20 bills.  Then, I tried to deposit the same cash back into our main checking account.  I waited for a few seconds while the machine chewed on my money and decided to spit out a $20 bill that it couldn't accept - something to do with the validity of the bill.  Of course, I tried multiple times unsuccessfully, hoping each time to pass the validity check, but instead found that this darn machine wouldn't accept it's own cash!

![chase-long-line](https://cloud.githubusercontent.com/assets/177508/8562771/ee50f866-24fb-11e5-816d-d44ceafdf508.jpg)

So, I finally drove up to the bank parking lot and rushed into the lobby to find a line of 8 customers waiting to be served.  Things are not looking good at this point for a quick transfer of cash.  So, I go back outside and speed my car to the Chase business checking drive-through.  I impatiently explain my quandary to the attendant.

"Your ATM machine doesn't seem to want to accept cash that I received as a withdrawal from another account.  I don't know what the problem is.  Can you help me?"

She curtly took my $20 bill and ATM card (presumably because I am not a business customer) and deposited the money directly for me.  And when she came back to the microphone, she looked at me through the window and explained that while she was sorry for the mishap, the ATM machine belongs to another company and that they are looking to replace it soon. I said thanks and drove away a little confused.  

Exactly what does that mean that the ATM machine is not theirs?  It had their logo on it, and the software clearly displayed their brand name in its wizard to withdraw and deposit my cash.  

How is that supposed to be a comfort to me as a frustrated consumer?  All I care about is getting my money transferred quickly.

However, this incident got me thinking about how the same kind of thing can occur as we support our own software products and services.  

It's easy for the internal and external teams that develop and support the software to blame each other to the customer for what caused a business critical incident.  And there's plenty of reasons why.

Maybe a Windows Update patch to your servers caused your Windows service not to restart, leaving your customers high and dry.  Or maybe one of the nodes in your web cluster went down and the health check that should have signaled your load balancer to redirect traffic to another node didn't work.  Or maybe the third party service you use for some feature didn't live up to their service level agreements and were down during a critical time.  Or maybe the Product Owner didn't understand what was best for their customer and had the team create a feature that just doesn't find acceptance with them.

Whatever the reason for the failure, from the customer's perspective the product doesn't work.  And getting the problem resolved quickly should be the first order of business.  

It's not one particular group that is ultimately responsible.  Because when customers use our software, they only see one, unified product.  They don't understand the intricacies of how and why a problem occurred.  They only care that the problem occurred and that it needs to be fixed.

There will be time to get to the bottom of the root cause and figure out how to avoid it in the future, but for now, quality shows in how quickly you can respond and fix your customer's issues.  So, anything that you can do to make that process smooth and easy is recommended.  

So, think through what kinds of information your application should be emitting when trouble strikes and make sure that you have the proper tools, access, and support that you need to get to the bottom of an issue and enhance your application's maintainability.

And remember that quality is everyone's responsibility.