<img src="http://lostechies.com/gabrielschenker/files/2011/03/swiss-knife_2.jpg" width="175px" alt="Single Responsibility"></img>

Chapter 2 of Sandi Metz's ["Practical Object-Oriented Design in Ruby - An Agile Primer"](http://www.amazon.com/Practical-Object-Oriented-Design-Ruby-Addison-Wesley/dp/0321721330/ref=sr_1_1?ie=UTF8&qid=1404882955&sr=8-1&keywords=sandi+metz) is all about designing classes with single responsibility in mind.  As Sandi puts it,

>Design is more the art of preserving changeability than it is the act of achieving perfection.

Her definition of easy to change is as follows:

* Changes have no unexpected side effects
* Small changes in requirements have correspondingly small changes in the code
* Existing code should be easy to reuse
* The easiest way to make a change is to add code that in itself is easy to change

The following are corresponding recommendations that follow the acronym TRUE:

* **Transparent** - the consequences of change should be obvious in the code that is changing and in distant code that relies on it
* **Reasonable** - the cost of a change should be proportional to the benefit the change achieves
* **Usable** - existing code should be usable in new and unexpected contexts
* **Exemplary** - the code itself should encourage those who change it to perpetuate those qualities

And what better way to embrace this kind of change than adhering to single responsibility within your class.  The more well-defined the responsibility is for a particular class, the easier to maintain because your application is more likely to:

* Reuse functionality - tighter, more focused responsibilities mean that reuse can happen at a granular level
* Avoid duplication - since code reuse will be more likely, the odds of having to duplicate code from another method or class is unlikely
* Not experience ripple effects when making changes - if classes only depend on you for a single responsibility, the only reason you would impact them would be for the functionality that they are leveraging.  And you are less likely to change since there should only be one and only one reason to change.

From a practical perspective, the author mentions that we can identify whether or not the single responsibility principle is being violated by doing one of two things:

1.  Interrogate the class by rephrasing it's methods as questions to see if they make sense. <p>**Example:**  Please Mr. Gear, what is your ratio?</p>
2. Attempt to describe what a class does in one sentence. <p>**Example:**   A gear is responsible for knowing how to calculate it's ratio.</p>

	
