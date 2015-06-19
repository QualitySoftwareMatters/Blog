![failing-fast](https://cloud.githubusercontent.com/assets/177508/8222371/f16bd2c2-152f-11e5-87fd-127fbfe055d4.png)

Raise your hand if you have seen this before in your production error logs.

<pre>System.NullReferenceException: Object reference not set to an instance of an object.</pre>

This is a tell-tale sign that a method or property on a null object within the body of a method has been called.  The stack trace can help us to locate where the error occurred.  However, an ambiguous error like this can be hard to troubleshoot for many reasons:

* If multiple local variables exist within the method, it can be hard to determine which one was null.
* If the variable was an input parameter, it can be hard to determine which method within the call stack the null reference was introduced.

In my [last post](http://www.qualitysoftwarematters.com/2015/06/what-is-software-quality.html), I talked about the different factors that define software quality.  One of those, maintainability, is very important once your application goes live.  Without it, it can be very difficult to troubleshoot issues when they occur.

The define:

Effort required to locate and fix an error in an operational program.

<pre lang="csharp">
public class ProgramWithoutFailFast
{
 	public static void Main(string[] args)
    	{
       	string firstName = null;
       	Console.WriteLine(GetAnswer(firstName));
    	}
    
	public string GetAnswer(string firstName)
    	{
       	return StartsWithT(firstName) ? "Yes" : "No";
    	}
    	
	public bool StartsWithT(string firstName)
    	{
        	return firstName.StartsWith(&quot;T&quot;)
	}
}
</pre>
