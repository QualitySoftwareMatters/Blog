I have been writing some integration tests in .NET lately to specify behavior for my data layer.  The issue that always comes up is how to make sure that each test is completely isolated from other tests.  This requires each test to initialize needed data and at the end to clean up any data that was created so that other tests are not impacted by it.  In the past I have set up compensating queries to delete that same data on tear down.  

This can create one of two problems:

* **maintainability** - it is hard to maintain this logic going forward
* **reliability** - it doesn't guarantee successful rollback of the initial inserts because the query could possibly fail leaving the tests in a position for unsuccessful future tests against the database

So, what do you do?

I have been using the handy [TransactionScope](http://msdn.microsoft.com/en-us/library/system.transactions.transactionscope.aspx) class to allow any connections to participate in the ambient transaction and then dispose of the transaction without committing on tear down of the fixture.  

In the example below, I have used NUnit, but this could work with other testing frameworks quite well.

	[TestFixture]
	public class MyFixture
	{
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
		
		///<summary>This is a silly sample test for display purposes only.</summary>
		[Test]
		public void given_some_context_when_something_happens_should_have_some_expected_outcome()
		{
			ExecuteSomeLogicForInsertingDataForContext();
			
			RunSomeActionToMakeSomethingHappen();
			
			AssertThatSomeExpectedOutcomeOccured();
		}
	}
	
You could make this an abstract base class and make the SetUp and TearDown methods virtual if you would like to reuse this across any of your data test fixtures.  As long as you don't call _scope.Complete() the changes you have made should be rolled back/aborted on the disposal of the transaction.

Hope this helps!