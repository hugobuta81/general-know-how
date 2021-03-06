# Loop through table rows without cursor
Article link:
* https://www.mssqltips.com/sqlservertip/6148/sql-server-loop-through-table-rows-without-cursor/

Example
```sql
-- It also demonstrate how to declare auto increament Id for virtual table
declare @tblItem as table (Id int not null identity(1,1), ItemName nvarchar(max))
insert into @tblItem (ItemName)
values ('Item 1'), ('Item 2'), ('Item 3'), ('Item 4'), ('Item 5')

declare @tblItemCnt int = 0;
declare @tblItemIndex int = 1;

declare @itemName nvarchar(20);

select @tblItemCnt = count(0) from @tblItem
while @tblItemIndex <= @tblItemCnt
begin
	set @itemName = (select ItemName from @tblItem where Id = @tblItemIndex)

	print 'Process Item: ' + @itemName

	-- REMEMBER to increase the index !!!
	set @tblItemIndex = @tblItemIndex + 1
end
```

# Execute dynamic query
Article links
* https://stackoverflow.com/questions/20678725/how-to-set-table-name-in-dynamic-sql-query

Example
```sql
set @sqlQuery = 'select Id from ' + @tableName + ' where Id = @ItemId'
set @paramDefinition = '@ItemId int'
execute sp_executesql @sqlQuery, @paramDefinition, @itemId
```

# Debug with print
```sql
select @tblItemCnt = count(0) from @tblItem
print '    [' + @tableName + '] total items to delete: ' + CAST(@tblItemCnt AS VARCHAR(MAX))
```

Convert datetime to string
https://www.sqlservertutorial.net/sql-server-system-functions/convert-datetime-to-string/
```
CONVERT(VARCHAR, datetime [,style])
```

# Remember to reset Identity when doing a purge
```sql
delete from TableOfSomething
DBCC CHECKIDENT ('TableOfSomething', RESEED, 0);
```

# Add days to current date
Article link
* https://www.w3schools.com/sql/func_sqlserver_dateadd.asp

```
DATEADD(day, 20, GETDATE())
```

# Get the Id of latest inserted row
Article link
* https://www.munisso.com/2012/06/07/4-ways-to-get-identity-ids-of-inserted-rows-in-sql-server/

```
INSERT INTO TableA (...) VALUES (...)
SET @LASTID = SCOPE_IDENTITY()
```
# Random integer number
https://www.techonthenet.com/sql_server/functions/rand.php#:~:text=To%20create%20a%20random%20integer,generate%20a%20random%20number%20for.
```
SELECT FLOOR(RAND()*(b-a+1))+a;
```
Where a is the smallest number and b is the largest number that you want to generate a random number for.
