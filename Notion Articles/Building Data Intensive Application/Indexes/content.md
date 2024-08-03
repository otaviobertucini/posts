- Indexes make reads faster, but makes writes slower
	- This because to organize date to be read faster, the database needs to use additional data structures
	- For each write, these data structures need to be updated, causing the writes to be slower
- The most common data structure used for indexing is a B-Tree
	- Balanced-Tree. With balanced we mean that all the leaf nodes are in the same level.
- An index is an ordered representation of stored data
	- Like in a phonebook, where we can say there is a index (last_name, first_name), numbers are ordered by the last name. So we can say there is an index in the last name
	- If we had to search for the first name, we would need to perform a full scan
	- On composite indexes, the order of the columns matter
- Developers should elaborate the indexes, not DBAs
	- This is because indexes depend on the context of the application
	- The developer knows the use cases of the application
- Indexes can be ordered
	- Default is ASC

	```sql
	-- Single column unique index with ascending order
	CREATE UNIQUE INDEX UQ_Products_ProductCode
	ON Products (ProductCode ASC);
	
	-- Composite unique index with different sort orders
	CREATE UNIQUE INDEX UQ_ProductSales
	ON ProductSales (ProductID ASC, SaleID DESC);
	```

- Indexes can be told which rows to be included in the index
	- If the column of the where clause changes to a value outside the constraint the row is removed

	```sql
	CREATE TABLE products (
	    id INT PRIMARY KEY,
	    product_name VARCHAR(255),
	    discontinued BOOLEAN
	);
	
	-- Create a partial unique index
	CREATE UNIQUE INDEX unique_active_product_name
	ON products (product_name)
	WHERE discontinued = FALSE;
	```

- If we use an indexed column in a where clause and other columns in the select clause the database engine will still have to go to the database to fetch the row
	- This is not bad if we are dealing with few fetched records, but if we use a BETWEEN clause that returns a lot of rows this additional lookup can cost a lot
		- In this case, a full table scan would be faster, because it can work in batches
- Aggressive indexes = index that are only used by one or a few queries; very specific indexes
- Using functions like MONTH, YEAR, LOWER obfuscates the indexes, meaning that the query will not be able to see the column passed to the function
	- To avoid this, one can create a computed column or create a functional index








