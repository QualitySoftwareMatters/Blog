![Quality](https://cloud.githubusercontent.com/assets/177508/7900064/c8847aac-0707-11e5-986f-abaa50651310.png)

What would you say if I asked you what are the marks of high quality software?  

Would you have answered, "a really nice/slick/sexy user interface?"  Or perhaps you would have said, "absence of known defects or bugs."  Those in the TDD camp might have replied, "thoroughly tested and validated with automation."  Or an architect might have preached about the importance of one or more of the -ilities.

### A History of Quality ###

Turns out that same question was asked of Jim McCall and his answer was captured in a [study he conducted for the Air Force in 1977](http://oai.dtic.mil/oai/oai?verb=getRecord&metadataPrefix=html&identifier=ADA049014).  What's surprising is that its findings are very similar to what we still claim as quality for applications today.

In his study on the subject, he initially identified 55 factors of quality.  In order to make the study more practical he reduced those 55 quality factors down to 11 and grouped them into 3 categories:  

* **Product operation** - how well it runs
* **Product revision** - how well it can be changed, tested, and deployed
* **Product transition** - how well it can be moved to other platforms and interface with other systems

The diagram below shows a visual representation of his findings.

![McCall's Quality Model](https://cloud.githubusercontent.com/assets/177508/8147733/9a45983a-1240-11e5-90fa-3067795dfc63.gif)

### Definitions of Quality Factors ###

Below are definitions of each of the quality factors.

* **Correctness** - Extent to which a program satisfies its specifications and fulfills the user's mission objectives.
* **Reliability** - Extent to which a program can be expected to perform its intended function with required precision.
* **Usability** - Effort required to learn, operate, prepare input, and interpret output of a program.
* **Integrity** - Extent to which access to software or data by unauthorized persons can be controlled.
* **Efficiency** - The amount of computing resources and code required by a program to perform a function.
* **Maintainability** - Effort required to locate and fix an error in an operational program.
* **Flexibility** - Effort required to modify an operational program.
* **Testability** - Effort required to test a program to insure it performs its intended function.
* **Portability** - Effort required to transfer a program from one hardware configuration and/or software system environment to another.
* **Reusability** - Extent to which a program can be used in other applications - related to the packaging and scope of the functions that programs perform.
* **Interoperability** - Effort required to couple one system with another.

### Today's View on Quality ###

This list should help you to think at a high level about the necessary factors involved with creating high quality software.  In more recent years, the literature on quality group the factors under either internal or external quality which are more concerned with the **_stakeholder_** rather than the **_product_**.

* **External Quality** - concerned with the **_end user's_** experience in **_using_** the product; these include all of the Product Operation factors
* **Internal Quality** - concerned with the **_developer's_** experience in **_maintaining_** the product; these include all of the factors listed in the Product Revision and Product Transition categories.   

While the external quality is of utmost importance in delighting customers, if internal quality factors are ignored, over time, delivering new external quality will be hampered.