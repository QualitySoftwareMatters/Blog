![readability-logo](https://cloud.githubusercontent.com/assets/177508/8350656/01b83e32-1aed-11e5-94bb-02a795ea57d3.png)

Is creating readable code an important feature of writing quality software?  

Is it something that you would mention when reviewing code for one of your team members?

Typically, when we review code we focus on making sure that:
	
1. It does what it is supposed to do
2. It handles potential failures
2. It avoids duplication
3. It performs well if it is computationally intensive

But when it comes to the readability of someone's code, we often don't want to mention it.  

Why?

For some, it might be related to fear of being accused of being petty or nit-picky.  Others might not want to stifle a fellow developer's personal expression or creativity.  And others might be more focused on getting things done and feel that focusing on readability is a waste of valuable developer time.

## Is Readability Important?  ##

Today, I would like to suggest that readability is important.  And, I will start by providing a couple of code samples to demonstrate my point.  

Take a look at the following SQL code and think about how long it takes you to figure out what is going on.

<pre lang="tsql">
SELECT * FROM dbo.MyTable WHERE ((PATINDEX('%[0-9]%', @Filter) = 0) AND (@Filter  LIKE '%,%'))
</pre>

Now, take a look at the same code that has been made more readable.

<pre lang="tsql">
SELECT	*
FROM	dbo.MyTable
WHERE	(
		(@filterContainsNumbers = 1) 
		AND 
		(@filterContainsComma = 1)
	)
</pre>

By using the simple concepts of **reader-friendly layout**./#Use Reader-Friendly Layout] and **intention revealing variable names**, someone can quickly scan your code and understand it within a matter of seconds.  The first example would probably take a few minutes to restructure the layout and look up the definition of the PATINDEX function online.

Take a look at another code example written in C#.

<pre lang="csharp">
public void RenderRemainingMoop(Liability source)
{
	var remainingMoop = source.OopRemaining + source.OopApplied;
	return remainingMoop == 9999999999 ? String.Empty : OopRemaining.ToString("{0:c}");
}
</pre>

Now take a look at the more readable version.

<pre lang="csharp">
public void RenderRemainingMaxOutOfPocket(Liability liability)
{
	const LiabilityMaxNumber = 9999999999;
	var remainingMaxOutOfPocket = liability.OutOfPocketRemaining + liability.OutOfPocketApplied;
	
	return remainingMaxOutOfPocket == LiabilityMaxNumber 
		? String.Empty
		: OutOfPocketRemaining.ToString("{0:c}");
}
</pre>

In this example, the layout again helps with reader comprehension.  In addition, using variables to provide context for **magic numbers/values** and **avoiding the use of abbreviations** can better reveal what code is doing.

## The Golden Rule ##

In both of the readable code samples, the developer focused not only on getting the job done, but also on making sure that future readers of their code would easily comprehend it.  

And why is this important?  

Because it makes the code more **_maintainable_** and **_flexible_**.  

As I mentioned in a [previous post](http://www.qualitysoftwarematters.com/2015/06/what-is-software-quality.html) on software quality, **_maintainability is the effort required to locate and fix an error in an operational program_**.  If our code is more readable, then developers will be able to quickly understand it, which translates into quicker fixes.  In that [same article](http://www.qualitysoftwarematters.com/2015/06/what-is-software-quality.html), I also mentioned that **_flexibility is the effort required to modify an operational program_**.  When developers need to add functionality to existing code, making it more readable will also allow them to quickly get up to speed and know where to place their functionality.

![the-golden-rule](https://cloud.githubusercontent.com/assets/177508/8352484/189c3a3c-1afb-11e5-966d-073bf89d4921.jpg)

In both situations, you will be making someone else's job easier.  As the golden rule states, 

>Do unto others what you would have them do unto you.

You never know when that someone else might be you!

## Practicing the Golden Rule ##

So, what can we do to make our code more readable for others?

### Use Intention Revealing Names ###

Everything in software has a name, whether it is a variable, a function, a class, a package, a table, a service, etc.  And every time you have to decide on a name, you have the opportunity to reveal what the code is doing or means.

* Be Descriptive

When naming things, be as descriptive as possible.  And don't be afraid to use long names.  Modern languages these days don't have many constraints when it comes to naming variables or files, and the more descriptive, the more context for the reader.  There is no excuse for using abbreviations anymore.

Also, when using magic numbers or strings, make sure to assign them to a constant in order to provide a name that will give the reader a clue as to what business purpose that value provides.  It helps to eliminate some of the magic.

* Be Specific 

If you are having a hard time trying to name a class, function, or variable, it might indicate that you are not being specific enough in your code.  Perhaps a variable or function has multiple purposes that need to be split out in order to better define their responsibilities.  This could be related to a violation of the [single responsibility principle]().

* Be Ubiquitous

In Eric Evan's ground-breaking book, <a href="http://www.amazon.com/gp/product/0321125215/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0321125215&linkCode=as2&tag=meinershagenf-20&linkId=6CZABSYJYOAQGC7U">Domain-Driven Design: Tackling Complexity in the Heart of Software</a><img src="http://ir-na.amazon-adsystem.com/e/ir?t=meinershagenf-20&l=as2&o=1&a=0321125215" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />, he establishes the concept of [ubiquitous language](http://martinfowler.com/bliki/UbiquitousLanguage.html) in which every application should establish a common domain-specific language that is understood and used by both developers and business people.  This helps to reduce the cognitive friction of having to translate concepts from code to real-life and back again.  When you are naming, make sure to use the ubiquitous language then to avoid this kind of friction.  It will help reduce misunderstandings in the future.  

* Avoid Data Types in Names

In the past, using Hungarian notation where you prepended an abbreviated prefix to signal a variable's data type (ie. - iCounter, bIsValid, txtFirstName, etc.) was in vogue.  That only serves to obscure code by mixing the semantic meaning of a variable and its underlying data type.  Modern development environments can easily reveal the type of a variable by placing your cursor over the variable during debugging.

* Avoid Comments

Sometimes the use of comments can indicate a poorly named function or variable.  Instead of using comments to explain yourself, try to include that context within the name of the variable or function.  In some cases, comments cannot be avoided, but if you can, it will ensure that this documentation will not get out of sync with your code, as comments are so fond of doing.
		
### Use Reader-Friendly Layout ###

As shown earlier, laying out your code can make a big difference in how quickly a fellow developer (or even you, if you haven't seen the code in months) can understand your code.  

* Show Logical Structure

Layout gives clues as to the logical structure of your code and its flow.  In crafting your code, you can emphasize hierarchical logic by leveraging indentions and new lines.

<pre lang="tsql">
IF (@filter LIKE '%to%') BEGIN SELECT * FROM dbo.MyTable END
ELSE BEGIN SELECT * FROM dbo.MyOtherTable END
</pre>

<pre lang="tsql">
IF (@filter LIKE '%to%')
	BEGIN
		SELECT	*
		FROM	dbo.MyTable
	END
ELSE
	BEGIN
		SELECT	*
		FROM	dbo.MyOtherTable
	END
</pre>

* Avoid Run-on Sentences

Also, just as in writing, you want to avoid the use of run-on sentences in your code.  For example, instead of putting everything on one line as in the following example:

<pre lang="csharp">
var customers = eventsRepository.GetAll().Where(e => e.Date > DateTime.Now).Select(e => new Customer(e.FirstName, e.LastName));
</pre>

you can do the following:

<pre lang="csharp">
var customers = eventsRepository
	.GetAll()
	.Where(e => e.Date > DateTime.Now)
	.Select(e => new Customer(e.FirstName, e.LastName));
</pre>

Or, instead of doing the following:

<pre lang="csharp">
var customer = GetOrderFor(GetCustomerFor(HttpContext.Current.User.Identity.Name), Convert.ToInt32(Session["orderId"]));
</pre>

you might want to do the following:

<pre lang="csharp">
var username = HttpContext.Current.User.Identity.Name;
var customer = GetCustomerFor(username);
var orderId = Convert.ToInt32(Session["orderId"]);
var order = GetOrderFor(customer, orderId);
</pre>

While there may not always be universally accepted practices when it comes to layout, each team should decide on some rules for code formatting and stick to them.  Static code analysis tools can help to enforce the use of formatting rules and many editors today including add-ons that you can purchase provide the ability to have code automatically formatted as it is being created.

### Handling Conditionals

* Encapsulate Conditional Logic
* Negative Conditionals

For more information on making your code readable, check out Bob Martin's book, <a href="http://www.amazon.com/gp/product/0132350882/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0132350882&linkCode=as2&tag=meinershagenf-20&linkId=V5RP3A3INHPV7BIR">Clean Code: A Handbook of Agile Software Craftsmanship</a><img src="http://ir-na.amazon-adsystem.com/e/ir?t=meinershagenf-20&l=as2&o=1&a=0132350882" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

