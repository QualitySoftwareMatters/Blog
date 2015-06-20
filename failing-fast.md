![failing-fast](https://cloud.githubusercontent.com/assets/177508/8222371/f16bd2c2-152f-11e5-87fd-127fbfe055d4.png)

Raise your hand if you have seen this before in your development or production error logs.

<pre>System.NullReferenceException: Object reference not set to an instance of an object.</pre>

As you might have guessed, this is informing us that we are calling a method or property on an object that is currently null.  The stack trace might be able to help us to locate where the error occurred.  However, an ambiguous error like this can be hard to troubleshoot for multiple reasons.

* If the stack trace does not have line numbers and multiple local variables exist within the method, it can be hard to determine which variable was null.

<pre lang="csharp">
public void Method()
{
	var person1 = new Person{ FirstName = "Todd", LastName = "Meinershagen" };
	var person2 = new Person();

	Console.WriteLine(person1.LastName);
	Console.WriteLine(person2.FirstName); //kapow!
}

public class Person
{
	public string FirstName { get; set; }
	public string LastName { get; set; }
}
</pre>

* If the variable was an input parameter, it can be hard to determine which method within the call stack the null reference was introduced.

<pre lang="csharp">
public void Method1()
{
	Person person = null;
	Method2(person);
}

public void Method2(Person person)
{
	Method3(person);
}

public void Method3(Person person)
{
	Console.WriteLine(person.FirstName); //kapow!
}
</pre>

### Failing Fast Enhances Maintainability ###

In my [last post](http://www.qualitysoftwarematters.com/2015/06/what-is-software-quality.html), I talked about the various factors that define software quality.  One of those, maintainability, is very important, because it reduces **_the effort required to locate and fix an error in an operational program_**.  Using a technique like **_failing fast_** can enable you and your team to locate errors more quickly in both development and production.

So, what does it mean to fail fast?

It means that rather than write code that ignores or band-aids an issue (like setting default values) and allows the code to limp along throughout the execution of your program, you fail "immediately and visibly" the minute that you are aware that there is an issue. 

### Assertions are the Key ###

![assert-sign](https://cloud.githubusercontent.com/assets/177508/8266261/f42fbae4-16e9-11e5-9e71-94e87b7bb543.jpg)

In order to fail fast, the key is to use assertions in your code.  An assertion is code that checks for a condition, and if the condition is not met, fails.  In the case of the null object reference, you can do the following:

<pre lang="csharp">
public void Method(int id)
{
	var person = _gateway.GetPerson(id);

	if (person == null)
	{
		throw new ArgumentNullException("person");
	}

	Console.WriteLine(person1.LastName);
	Console.WriteLine(person2.FirstName);
}

public class Person
{
	public string FirstName { get; set; }
	public string LastName { get; set; }
}
</pre>

For null references, I like to check in places where two methods or classes interact:

* **Constructor**:  verify any dependencies passed in by another class
* **Method**:  check any input parameters passed in by another method or class
* **Method**:  check the response from another method or class, as in the case with the Gateway call above

### Drying Up Your Assertions ###
After using these kind of assertions throughout your code, you would want to DRY (don't repeat yourself) your code and create reusable assertions.  Below is one example.

<pre lang="csharp">
public class Assert
{
	public static void IsNotNull(object value, string paramName)
	{
		if (value == null)
		{
			throw new ArgumentNullException(paramName);
		}
	}
}
</pre>

You might also look into third party libraries such as [Magnum by Chris Patterson](http://www.nuget.org/packages/Magnum/) that are available as NuGet packages.  Below is an example using the Guard class that Magnum provides for these kind of common assertions.

<pre lang="csharp">
public void Method(int id)
{
	var person = _gateway.GetPerson(id);

	Guard.AgainstNull(person, "person");

	Console.WriteLine(person1.LastName);
	Console.WriteLine(person2.FirstName); //kapow!
}

public class Person
{
	public string FirstName { get; set; }
	public string LastName { get; set; }
}
</pre>

Another option, if you are using the .NET framework, is [Microsoft's Code Contracts](http://research.microsoft.com/en-us/projects/contracts/).  These allow the developer to specify pre- and post-conditions that can also be seen in the documentation of a given method.  A discussion of their use is outside the scope of this article.

### Is Failing Fast Robust? ###
At this point, some may object and say this technique of failing fast will create fragile software.  However, by failing fast, errors will more likely be found during development and testing of your software, rather than in production where it really counts.  And if a bug does escape in production, your team will more likely be able to fix the issue quickly by failing closer to where the issue originally occurred.

Another objection to this style of programming is that it will more likely cause the system to crash in front of the user.  One way to mitigate this is to use a global error handler that gracefully displays a user-friendly, generic error message to the user while providing the error details to developers through logs or email.  In the case of non-interactive applications such as batch processes or windows services, you don't have to display a message, but after handling the error globally, operations continue by moving on to the next action/transaction. 

### Other Resources ###
If you are interested in reading more about the concept of failing fast, check out James Shore's [article for IEEE magazine](http://www.martinfowler.com/ieeeSoftware/failFast.pdf) discussing the same topic.