Applications that rely on the DBMS to manage data run as separate processes that connect to the DBMS to interact with it.

  

  

### Accessing Databases from Applications

- SQL commands can be executed from within a program in a host language such as C or Java. The use of SQL commands within a host language program is called Embedded SQL.

  

### Embedded SQL

- SQL statements can be used wherever a statement in the host language is allowed. SQL statements must be clearly marked so that preprocessor can deal with them before invoking the compiler for the host language. Also, any host language variables used to pass arguments into an SQL command must be declared in SQL. In particular, some special host language variables must be declared in SQL.
- There are two complications to bear in mind.
    - data types recognized by SQL may not recognized by the host language and vice versa. This mismatch is typically addressed by casting data values appropriately before passing them to or from SQL commands.
    - SQL is set-oriented, and is addressed using cursors.

  

  

### Cursors

- programming languages like C do not cleanly support a set-of-records abstraction. The solution is to essentially provide a mechanism that allows us to retrieve rows one at a time from a relation.
- This is called a cursor. Once a cursor is declared, we can open it (which positions the cursor just before the first row); fetch the next row; move the cursor; or close the cursor.
- A cursor essentially allows us to retrieve the rows in a table by positioning the cursor at a particular row and reading its contents.
- A cursor can be thought of as pointing to a row in the collection of answers to the query associated with it. When a cursor is opened, it is positioned just before the first row.

  

  

## Dynamic SQL

- some applications must accept commands from a user and, based on what the user needs, generate appropriate SQL statements to retrieve necessary data. In such situations, we may not be able to predict in advance what SQL statements need to be execute, even though there is (presumably) some algorithm by which the application can construct the necessary SQL statements.
- PREPARE and EXECUTE are the main commands.