### Prompt Types
1. Debugging prompts
	- Instructions**: Include a query** in the prompt and ask the model to debug it. Ensure that the code contains at least one bug preventing correct execution or output generation.
	- **Note:** The bug in your query should not be too simple. Bugs like a single missing comma in a 7-line query won’t be accepted. Make them a bit more complex, such as calling the wrong function or table, using incorrect syntax within a function, structural mistakes, etc. Be creative.
	- Template:
		- [Why you want the query, context (brief story)]
		- [What the query is for]
		- [How the query works]
		- [What the query is doing right now]
		- [What you want the query to do]
		- [query inside a code block]
		- Make sure the error is not a simple syntax error, it should be something logical
	- Example:
			- I am a database manager working on bringing all of my company's systems up to date. I am trying to write a query to output XYZ by looking into tables ZYX. It does this by analyzing the Z table and joining it to the Y table, the grouping by X. Right now I believe there is an issue with CTE Y. The query is currently outputting ABC. Can you fix this query so it outputs XYZ?
```SQL
WITH product_category_sales AS ( 
SELECT pc.Name AS CategoryName, SUM(sod.LineTotal) AS TotalSales 
FROM Production.Product p 
JOIN Production.ProductSubcategory ps ON p.ProductSubcategoryID = ps.ProductSubcategoryID 
JOIN Production.ProductCategory pc ON ps.ProductCategoryID = pc.ProductCategoryID 
JOIN Sales.SalesOrderDetail sod ON p.ProductID = sod.ProductID 
JOIN Sales.SalesOrderHeader soh ON sod.SalesOrderID = soh.SalesOrderID 
GROUP BY pc.Name 
) 
SELECT CategoryName, TotalSales FROM product_category_sales ORDER BY TotalSales DESC;
```
		
			
2. Editing and Documentation Prompts 		
	- Instructions: Request the model to edit or document a query provided by you thoroughly**.** 
	- **Note:** You must not change the logic of the query.
	- Examples:
		- Include comments within the query to clarify its intention or functionality.
		- Create documentation to detail the usage and purpose of the code.
	- Template:
		- [Why you want the query, context (brief story)]
		- [What the query is for]
		- [How the query works]
		- [What the output of the query is]
		- [How you would like the query documented]
			- Adding comments, where? 
			- Summarizing the query into a write-up
			- Explaining what each line does
			- Etc. 
		- [How you would like the query editing]
			- Adding enters after each FROM statement
			- Capitalizing table names
			- Using CTEs instead of subqueries
			- **Do not change the output of the query**
	- Example: 
		- My local library needs help understanding their database after their IT team left the organization. The following query uses two CTEs to output orders total sales and group them by region and product. I would like to send this query to the new team. Could you please comment the query, summarizing the purpose at the top and adding inline comments that explain the role of each CTE?
```SQL
WITH customer_sales AS (
SELECT c.CustomerID, SUM(sod.unitprice * sod.orderqty) AS TotalSales
FROM Sales.salesorderheader soh
JOIN Sales.salesorderdetail sod ON soh.SalesOrderID = sod.SalesOrderID
JOIN Sales.Customer c ON soh.CustomerID = c.CustomerID
GROUP BY c.CustomerID
)

SELECT CustomerID, TotalSales
FROM customer_sales
ORDER BY TotalSales DESC;
```
3. Precise Instruction Following Prompts
	- Instructions: Provide a variety of very specific instructions for the model to follow.
	- Examples:
		- The query must consist of exactly N lines.
		- Rename all fields to names XYZ, ZYX, and GHJ.
		- Ensure the code follows a specific query style.
		- The query output should have a set number of rows and columns.
	- Template:
		- [Why you want the query, context (brief story)]
		- [What the query is for]
		- [How you want the query to work]
		- [What the output of the query is]
		- [very specific instructions, be creative]
			- The query must consist of exactly N lines.
			- Rename all fields to the names XYZ, ZYX, and GHJ.
			- Ensure the code adheres to a specific coding style; for example, how variables are named or whether to include a line break after a FROM statement.
			- Preferring JOIN or Subquery, multiple CTEs, or nested Subqueries.
	- Example:
		- I'm writing a query to assist a dog walking company analyze the speed of different dog breeds. Pull XYZ from ZYX, group by F, and perform analyses A, B, and C. When you do this:
			- Make the query at least 15 lines
			- Do not use subqueries
			- name all the fields as variations of different dog breeds.
			[No query required]
4. Other
	- Instructions: Write a complex prompt involving SQL and analyze it.
	- Example: Thoroughly read through the document to ensure all aspects of the SQL query are covered, including edge cases, optimization, and performance considerations.
	- Template:
		- [Why you want the query, context (**brief** story)]
		- [What the query is for]
		- [What the output of the query is]
		- [very specific instructions, be creative]
			- Request A, what it’s for, and what you want it to achieve. Specifics on how this can be found. **Be specific**
			- Request B, what it’s for, and what you want it to achieve. Specifics on how this can be found. **Be specific**
			- Request C, what it’s for, and what you want it to achieve. Specifics on how this can be found. **Be specific**
	- Example:
		- As part of the New Movie Store's efforts to enhance strategic marketing and customer engagement, develop a SQL query that provides a comprehensive analysis of customer behaviors and film popularity. The analysis should help the management to understand various dimensions of the business operations. Your SQL query should provide the following:
		1. Customer Value Segmentation: Identify the top 10 customers based on their total spending on rentals and purchases. Provide their names, total expenditure, and the count of transactions they have completed.
		2. Film Rental Trends: Calculate the monthly rental count for the most popular genre of the last year. Break down the counts month by month.
		3. Inventory Efficiency: Determine which store location hast the highest turnover rate of film rentals, defined as the total number of rentals divided by the number of inventory items available in that store.

### Just like SQL, Prompts Come in Multiple Flavors

**This course will be on the following**:

Every Task will have a defined Prompt Type

1. **Prompt Types**
2. 🐛 Debugging
3. 📃 Editing and Documentation
4. 📐 Precise Instructions Following
5. 🧮 Other
6. **How to write a prompt**
7. Guidelines

**A good prompt contains the following:**

1. Realistic Context
	- The background story for the query must make sense for the query requested. Imagine someone asked you for a query. At no point should you ask yourself "Why do they want this query? This is useless?"
2. Multiple requests for information that make sense together in a data science context
	- **[This does not always apply to precise instructions following]** PIF requests are less related to data science and more about how to structure the query.
	- **For Editing and Documentation:** Provide the model with a complex query with multiple opportunities to explain and comment on the query.
	- **Debugging:** Add multiple reasonable mistakes. These mistakes cannot simply be typos or syntax. Mistakes should be structural.
	- **Other:** Add multiple requests that make sense for the context and satisfy complexity requirements
![[Pasted image 20240614125956.png]]
![[Pasted image 20240614130012.png]]
- **✅Stick to Familiar Fields**: Ask questions related to fields you are familiar with. For example, if you are not well-versed in algorithms, avoid algorithm-related questions as verifying responses may be difficult.
- **✅Verify Originality**: Even if your prompt is original, paste it into a search engine to check if something similar appears in the results.
- **✅Increase Complexity**: Raise the complexity of a prompt by considering its use case. Create fake scenarios, add interesting constraints, test cases, and parameters to challenge the model’s problem-solving abilities.

**⛔DONT**

- ⛔**Do not have Predetermined Answers**: Do not use prompts with predetermined answers or plant bugs/errors to see if the model catches them all. This approach is impractical. 
	- _Check out the extended list of_ [**_Prompts to Avoid_**](https://docs.google.com/document/d/1PJneRCOKk3noFmvLsodRBthx7glKj80Hwuh3bhNNAoU/edit?usp=sharing) _here_

- ⛔Please **avoid** the following categories because they have a high probability of being flagged as too standard or too easy: 
	- **Basic Academic Exercises, such as:**
		- Simple SELECT queries without joins or subqueries (e.g., selecting all rows from a table)
		- Basic INSERT, UPDATE, DELETE operations on single tables
		- Fundamental aggregate functions without GROUP BY (e.g., calculating the total sum or average)
	- **Common SQL Problems, such as:**
		- Standard data retrieval tasks (e.g., finding the highest/lowest value in a column)
		- Basic join operations between two tables without conditions (e.g., inner join between two simple tables)
		- Elementary window functions (e.g., calculating running totals or row numbers)
	- **Overly Simplistic Projects, such as:**
		- Elementary data transformation tasks without handling nulls or duplicates
	- **Overused Interview Questions, such as:**
		- Classic SQL puzzles (e.g., finding the second-highest salary in a table)
		- Standard normalization or denormalization tasks
		- Common string manipulation tasks (e.g., using LIKE for pattern matching, simple REPLACE functions)