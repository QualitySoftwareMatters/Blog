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

# Is Readability Important?  #

Today, I would like to suggest that readability is important.  And, I will start by providing a couple of code samples to demonstrate my point.  

Take a look at the following SQL statement and think about how long it takes you to figure out what it does.

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

By using the simple concepts of **using reader-friendly layout** and **using intention revealing variable names**, someone can quickly scan your code and understand it within a matter of seconds.  The first example would probably take a few minutes to restructure the layout and look up the definition of the <code lang="tsql">PATINDEX</code> function.

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

In addition to layout, using variables to **provide context for magic numbers and strings** and **being descriptive** instead of using abbreviations can better reveal what code is doing.

# The Golden Rule #

In both of the readable code samples, the developer focused not only on getting the job done, but also on making sure that future readers of their code would easily comprehend it.  

And why is this important?  

Because it makes the code more **_maintainable_** and **_flexible_**.  

As mentioned in a [previous post](http://www.qualitysoftwarematters.com/2015/06/what-is-software-quality.html) on software quality, **_maintainability is the effort required to locate and fix an error in an operational program_**.  If your code is more readable, developers will be able to quickly understand it, which translates into quicker fixes.  In that [same article](http://www.qualitysoftwarematters.com/2015/06/what-is-software-quality.html), it was also mentioned that **_flexibility is the effort required to modify an operational program_**.  When developers need to add functionality to existing code, making it more readable will also allow them to quickly get up to speed and know where to place their functionality.

![the-golden-rule](https://cloud.githubusercontent.com/assets/177508/8352484/189c3a3c-1afb-11e5-966d-073bf89d4921.jpg)

In both situations, you will be making someone else's job easier.  As the golden rule states, 

>Do unto others what you would have them do unto you.

You never know when that someone else might be you!

# Practicing the Golden Rule #

So, what can we do to make our code more readable for others?

## Using Intention Revealing Names ##

Everything in software has a name.  And every time you name something, you have the opportunity to reveal its intention.

### Be Descriptive ###

When naming things, be as descriptive as possible, and don't be afraid to use long names.  Modern languages don't have many constraints when it comes to naming things, and the more descriptive, the better for the reader.  There is no excuse for using abbreviations.

### Provide Context for Magic Numbers and Strings ###

When using magic numbers or strings, make sure to assign them to a constant with a meaningful name.  It helps to dispel the magic.

### Be Ubiquitous ###

<a href="http://www.amazon.com/gp/product/0321125215/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0321125215&linkCode=as2&tag=meinershagenf-20&linkId=6CZABSYJYOAQGC7U"><img border="0" src="http://ws-na.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=0321125215&Format=_SL250_&ID=AsinImage&MarketPlace=US&ServiceVersion=20070822&WS=1&tag=meinershagenf-20" ></a><img src="http://ir-na.amazon-adsystem.com/e/ir?t=meinershagenf-20&l=as2&o=1&a=0321125215" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

In Eric Evan's ground-breaking book, <a href="http://www.amazon.com/gp/product/0321125215/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0321125215&linkCode=as2&tag=meinershagenf-20&linkId=6CZABSYJYOAQGC7U">Domain-Driven Design: Tackling Complexity in the Heart of Software</a><img src="http://ir-na.amazon-adsystem.com/e/ir?t=meinershagenf-20&l=as2&o=1&a=0321125215" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />, he establishes the concept of [ubiquitous language](http://martinfowler.com/bliki/UbiquitousLanguage.html), which establishes a common set of terms that can be used by both developers and business people alike.  This helps to reduce the cognitive friction and potential confusion of having to translate between business concepts and code.  

When you are naming something, prefer to use your domain's ubiquitous language rather than technical jargon.  It will help reduce time and misunderstandings in the future.  

### Avoid Data Types in Names ###

In the past, using Hungarian notation to prepend an abbreviated data type to a variable name (**examples**:  iCounter, bIsValid, txtFirstName, etc.) was in vogue, but it obscures the name by mixing the semantic meaning of a variable and its underlying data type.  Modern development environments can easily reveal the type of a variable by hovering over the variable during debugging.
		
## Using Reader-Friendly Layout ##

As shown earlier, laying out your code can make a big difference in how quickly a fellow developer (or even you, if you haven't seen the code for a while) can comprehend your code.  

### Show Logical Structure ###

Layout your code to emphasize the logical structure by using indentations and new lines.  The examples below demonstrate this transformation.

<pre lang="tsql">
--BEFORE
IF (@filter LIKE '%to%') BEGIN SELECT * FROM dbo.MyTable END
ELSE BEGIN SELECT * FROM dbo.MyOtherTable END
</pre>

<pre lang="tsql">
--AFTER
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

### Avoid Run-on Sentences ###

Just as in good writing, you want to avoid the use of run-on sentences in your code.  Instead of putting everything on one line as in the following example that uses a fluent syntax:

<pre lang="csharp">
//BAD
var customers = eventsRepository.GetAll().Where(e => e.Date > DateTime.Now).Select(e => new Customer(e.FirstName, e.LastName));
</pre>

you can do the following:

<pre lang="csharp">
//BETTER
var customers = eventsRepository
	.GetAll()
	.Where(e => e.Date > DateTime.Now)
	.Select(e => new Customer(e.FirstName, e.LastName));
</pre>

Or, instead of nesting multiple statements into one line for efficiency:

<pre lang="csharp">
//BAD
var customer = GetOrderFor(GetCustomerFor(HttpContext.Current.User.Identity.Name), Convert.ToInt32(Session["orderId"]));
</pre>

you might want to do the following:

<pre lang="csharp">
//BETTER
var username = HttpContext.Current.User.Identity.Name;
var customer = GetCustomerFor(username);
var orderId = Convert.ToInt32(Session["orderId"]);
var order = GetOrderFor(customer, orderId);
</pre>

While there may not always be universally accepted practices when it comes to layout, each team should decide on rules for code formatting and stick to them.  Static code analysis tools can help to enforce those rules and modern editors include utilities for automatically formatting code according to your rules as it is being written.

## Handling Conditionals ##

Another area where code can become unreadable is when using boolean logic in <code lang="sharp">if</code> and <code lang="sharp">while</code> statements.  

### Extracting Conditional Logic ###

For example, while the following code sample is fairly readable, it can be improved.

<pre lang="csharp">
if (rolls[frameIndex] + rolls[frameIndex + 1] == 10) // spare
{
	score += 10 + rolls[frameIndex + 2];
} 
else 
{
	score += rolls[frameIndex] + rolls[frameIndex + 1];
}
</pre>

Simply extract the logic contained in the boolean expression to another method, allowing the reader to understand the conditional logic from a conceptual point of view - in this case bowling a spare.

<pre lang="csharp">
if (IsSpare(frameIndex))
{
	score += 10 + spareBonus(frameIndex);
} 
else 
{
	score += 10 + sumOfBallsInFrame(frameIndex);
}
</pre>

Most modern development environments have built-in refactoring utilities that make this kind of operation trivial.

### Negative Conditionals ###

Negative conditional logic can also obscure the meaning of an expression by creating cognitive dissonance that requires the reader to interpret.  This wasted time could be alleviated by re-stating the condition in positive terms by creating a method to return the opposite of the intended expression.

For example,   

<pre lang="sharp">
if (!String.IsNullOrEmpty(value))
	doSomething();
</pre> 

could very easily have been written as the following.

<pre lang="sharp">
if (ContainsText(value))
	doSomething();
</pre> 

<a href="http://www.amazon.com/gp/product/0132350882/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0132350882&linkCode=as2&tag=meinershagenf-20&linkId=V5RP3A3INHPV7BIR"><img border="0" src="http://ws-na.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=0132350882&Format=_SL250_&ID=AsinImage&MarketPlace=US&ServiceVersion=20070822&WS=1&tag=meinershagenf-20" ></a><img src="http://ir-na.amazon-adsystem.com/e/ir?t=meinershagenf-20&l=as2&o=1&a=0132350882" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

For more information on making your code readable, check out Bob Martin's book, <a href="http://www.amazon.com/gp/product/0132350882/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0132350882&linkCode=as2&tag=meinershagenf-20&linkId=V5RP3A3INHPV7BIR">Clean Code: A Handbook of Agile Software Craftsmanship</a><img src="http://ir-na.amazon-adsystem.com/e/ir?t=meinershagenf-20&l=as2&o=1&a=0132350882" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />