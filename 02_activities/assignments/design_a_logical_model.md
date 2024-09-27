# Assignment 1: Design a Logical Model

## Question 1
Create a logical model for a small bookstore. 📚

At the minimum it should have employee, order, sales, customer, and book entities (tables). Determine sensible column and table design based on what you know about these concepts. Keep it simple, but work out sensible relationships to keep tables reasonably sized. Include a date table. There are several tools online you can use, I'd recommend [_Draw.io_](https://www.drawio.com/) or [_LucidChart_](https://www.lucidchart.com/pages/).

Relationships:
Employee → Order: An employee processes an order (EmployeeID is a foreign key in the Order table).
Customer → Order: A customer places an order (CustomerID is a foreign key in the Order table).
Order → Sales: Each sale is linked to an order (OrderID is a foreign key in the Sales table).
Book → Sales: Each sale refers to a specific book (BookID is a foreign key in the Sales table).
Date → Order, Sales: Both orders and sales are linked to specific dates (OrderDate, SaleDate link to DateID in the Date table).




## Question 2
We want to create employee shifts, splitting up the day into morning and evening. Add this to the ERD.

Updated Relationships:
Employee → EmployeeShift: Each employee can be assigned to multiple shifts (EmployeeID is a foreign key in the EmployeeShift table).
Date → EmployeeShift: Each shift occurs on a particular date (DateID is a foreign key in the EmployeeShift table).

This new table integrates into the existing model by assigning shifts to employees and linking shifts to specific dates, ensuring that morning and evening shifts are clearly identified.




## Question 3
The store wants to keep customer addresses. Propose two architectures for the CUSTOMER_ADDRESS table, one that will retain changes, and another that will overwrite. Which is type 1, which is type 2?

_Hint, search type 1 vs type 2 slowly changing dimensions._

Tables' Explanation:
1. Type 1 SCD (Overwrites Address Changes)
In this architecture, the CUSTOMER_ADDRESS table will store only the most recent address, overwriting any previous addresses whenever a change is made.
In this design, every time a customer's address changes, the old address is overwritten by the new one. This is Type 1 SCD because it only keeps the current address and does not retain historical records of previous addresses.
2. Type 2 SCD (Retains Address Changes)
In this architecture, the CUSTOMER_ADDRESS table will retain the history of all address changes by creating a new record each time the customer updates their address. The current address will be indicated by an additional column.
In this design, each address change is recorded as a new row with a unique AddressID. The IsCurrent field indicates the most recent address, and StartDate/EndDate fields keep track of when the address was valid. This is Type 2 SCD because it retains all address changes as historical data.

Bonus: Are there privacy implications to this, why or why not?
Type 1 SCD: Since Type 1 only keeps the current address, it minimizes privacy concerns because no historical data is stored. However, if the system does not properly track changes or misuse occurs, overwriting data may hide sensitive address changes (e.g., if a customer moves to avoid an unsafe situation).
Type 2 SCD: Type 2 stores all previous addresses, raising more significant privacy concerns because historical records may reveal sensitive location changes. It's essential to ensure that this data is securely stored and access is properly controlled, as unauthorized access to old addresses could expose private information or patterns about the customer's movements.
Conclusion:
Type 1 SCD: Overwrites changes (simpler, but no history).
Type 2 SCD: Retains changes (more complex, but preserves history).





## Question 4
Review the AdventureWorks Schema [here](https://i.stack.imgur.com/LMu4W.gif)

The AdventureWorks schema differs from the small bookstore ERD in several ways. There are two key differences along with reflections on how these could impact or influence the small bookstore ERD:

1. Normalization and Use of Lookup Tables in AdventureWorks
AdventureWorks:
The AdventureWorks schema uses lookup tables to standardize and reference frequently repeated data (e.g., Person.AddressType or Person.PhoneNumberType).
It breaks down complex entities into smaller, more granular tables to promote normalization. For example, instead of having address fields directly in the Customer table, there’s a CustomerAddress table that joins with the Address table.
Bookstore ERD:
In the small bookstore ERD, all the customer address fields (e.g., City, State, PostalCode) are stored directly in the Customer or CustomerAddress table without additional lookup tables for common attributes such as city or postal code.
Reflection:
Normalization: AdventureWorks uses higher normalization levels to reduce redundancy and improve data integrity. To apply this to the bookstore ERD, we could introduce lookup tables for city, state, or country codes. However, for a small bookstore, the current design may be more efficient as it simplifies queries and reduces the need for joins, which could be unnecessary for a smaller dataset.

2. Product Inventory Granularity and Organization
AdventureWorks:
AdventureWorks includes more granular product tables, such as separating out Product, ProductInventory, and ProductCategory. This allows for detailed tracking of inventory, categories, and product relationships.
ProductInventory holds data on stock levels at specific warehouse locations.
Bookstore ERD:
In the bookstore ERD, we only have a Book table that combines book details and inventory count (StockQuantity). There is no separate Inventory table or warehouse management.
Reflection:
Inventory Management: The bookstore ERD could benefit from a more detailed inventory system, especially if the bookstore has multiple locations or distribution centers. For example, separating inventory into its own table would make it easier to track stock levels across different store branches, if applicable. However, for a small, single-location bookstore, the current setup might suffice, keeping the database simpler and more manageable.

Would I Change Anything?
For a small bookstore: I wouldn’t implement heavy normalization or separate inventory tables for a single-location store. The simpler design works well for operational efficiency and ease of use in a smaller system.
If the bookstore scales: As the business grows, we might adopt some of AdventureWorks’ strategies, such as separating inventory into a distinct table or introducing lookup tables for cities and states to enhance performance and consistency across larger datasets.







# Criteria

[Assignment Rubric](./assignment_rubric.md)

# Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `September 28, 2024`
* The branch name for your repo should be: `model-design`
* What to submit for this assignment:
    * This markdown (design_a_logical_model.md) should be populated.
    * Two Entity-Relationship Diagrams (preferably in a pdf, jpeg, png format).
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sql/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `model-design`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack at `#cohort-4-help`. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
