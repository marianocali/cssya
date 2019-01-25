###CREATING USER AND DATABASE IN MYSQL
Here is a description with command line you can do the same in Workbench as you choice.

1) At the command line, log in to MySQL as the root user:  

    mysql -u root -p


2) create a database user for the application with the values:   

* username: cpiusr      
* password: cpipwd  

        GRANT ALL PRIVILEGES ON *.* TO 'cpiusr'@'localhost' IDENTIFIED BY 'cpipwd';  

3) create the database  

    CREATE DATABASE enumeration;  

4) populate tables in order to have data to work  

* enumbe-model/sql/Initialdata.sql
* enumbe-model/sql/sectortypes.sql  
all files from folder:
* enumbe-model/sql/shopsdump

        mysql -u cpiusr -p enumeration < enumbe-model/sql/Initialdata.sql
        mysql -u cpiusr -p enumeration < enumbe-model/sql/sectortypes.sql

Make the same for the others files.

-----
### OPEN CODE AND MODIFYING CONFIGURATION FOR DEVELOP

5) import the source as maven project  
To generate the files needed for an IntelliJ IDEA Project setup, you only need to execute the main plugin goal, which is idea:idea like so:

    mvn idea:idea

6) change configuration in the file **enumerations.properties** for work with mysql  
    
jdbc.url=jdbc:jtds:sqlserver://localhost:1433/  
must be replaced by:  
jdbc.url=jdbc:mysql://localhost:3306/

And   
db.dialect=org.hibernate.dialect.SQLServerDialect  
must be replaced by:  
db.dialect=org.hibernate.dialect.MySQLDialect

7) in order to work with dev environment, in the **enumbe-main-ctx.xml uncomment** the following line:  
  
    import resource="classpath:spring/enumbe-db-ctx-DEV.xml"  
 and **comment** the following line  :

     import resource="classpath:spring/enumbe-db-ctx.xml

8) In the file **enum-backoffice/pom.xml **
in the properties section add the following  

    <activeProfile>enumbe-model</activeProfile>

9) Download and compiles all the needed components

    mvn install

10) to run the application in a terminal execute:

    mvn tomcat:run

11) if it is not possible to login in tomcat or postman when yo made a request do the following steps:  

a)Add a user into tblUser  

    insert into tblUser(accountNonExpired, accountNonLocked,credentialsNonExpired,enabled, isAdmin, username, password, name,assignedlocationID, deviceID)
    values (1,1,1,1,1,'1234','cpipass','cpiusr',7934,'fe692ac8cda70cff');

b) get the of new file added  

    select userID from tblUser where name like 'cpiusr';

c) insert into tblUser_GrantedAuthority a record in order to have integrity  

    insert into tblUser_GrantedAuthority
    values(userID, 1)
the userID must be the number get in the previous step

