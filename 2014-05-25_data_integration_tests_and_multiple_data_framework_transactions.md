Last time I talked about how to use the [TransactionScope](http://msdn.microsoft.com/en-us/library/system.transactions.transactionscope.aspx) class to handle the rollback of any changes made during a data integration test.  This time, I would like to talk about another issue that will eventually come up using transactions:  using [Entity Framework](http://msdn.microsoft.com/en-us/data/ef.aspx) code-first in combination with any other data access framework while leveraging TransactionScope.

In our case, we use [Dapper](https://code.google.com/p/dapper-dot-net/) to insert data before our tests and to assert things about the state of the database after exercising the [code-first Entity Framework](http://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx) data layer functionality we are testing.  Below is our example.

	private TransactionScope _scope;
	
	[SetUp]
	public void SetUp()
	{
		_scope = new TransactionScope();
	}
	
	[TearDown]
	public void TearDown()
	{
		_scope.Dispose();
	}
	
	[Test]
	public void test_to_demo_ef_and_dapper_connections()
	{
		var person = new Person {FirstName = "Todd", LastName = "Meinershagen"};
		var sql = "INSERT INTO dbo.Persons (FirstName, LastName) ";
		sql = sql + "VALUES (@FirstName, @LastName)";
		
		using (var connection = new SqlConnection("a connection string"))
		{
			connection.Execute(sql, person);
		}
		
		var db = new PersonContext();
		var matchingPersons = 
			from p in db.Persons
			where
				(p.FirstName == person.FirstName) &&
				(p.LastName == person.LastName)
			select p;
			
		matchingPersons.FirstOrDefault().Should().NotBeNull();
	}

Both Dapper and EF establish their own connections, and we would expect that each connection would take part in the ambient transaction being created during the [SetUp] of our test fixture.  

Normally, this is not an issue, but when the tests are run, we get a message similar to below:

	MSDTC on server 'servername' is unavailable.

This can occur for any number of reasons such as the following:

* Opening multiple connections with same connection string to SQL Server 2005.
* Opening multiple nested connections with same connection string to SQL Server 2008.
* Opening multiple connection to two different SQL Server 2008 instances.

In the case of our tests, we are making two connections (one for EF and one for Dapper) to SQL Server 2008 using the same connection string.  Based on the guidance above, this should not force our system to escalate to MSDTC.  

So, what is happening here?

After searching diligently on the internet (thank God for the internet!), I found an [article](http://stackoverflow.com/questions/18088949/entityframeworkmue-in-entity-framework) explaining that Microsoft cleverly adds information to a code-first connection string to allow Microsoft to collect statistics from those using Azure and Entity Framework to determine what percentage use code-first as opposed to database-first.  (Why Microsoft, why?)  This is supposed to have been fixed in EF 6.0.

So, instead of using a connection string as you specified:

	Data Source=(local);
	Initial catalog=LocalDb;
	Integrated Security=True;
	
The system uses the following for EF:

	Data Source=(local);
	Initial catalog=LocalDb;
	Integrated Security=True;
	Application Name=EntityFrameworkMUE

So, what does this mean?

Unfortunately, this causes our system to see the two connections (Dapper and EF) as two different connection strings and therefore, it looks like you are connecting to two different data sources which escalate to MSDTC.  What a pain!

So, how do we get around this issue.

One way would be to use EF for both production and test code, although this robs us of the benefit of quickly setting up data using a light-weight framework like Dapper.  Another option is to modify our test project's configuration file by explicitly specifying the Application Name for our connection string.  The system will then see the two connections as the same.  No more escalation to MSDTC!

Hope this helps.

