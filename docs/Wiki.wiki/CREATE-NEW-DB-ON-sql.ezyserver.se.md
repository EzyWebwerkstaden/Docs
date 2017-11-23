# Create new db on sql.ezyserver.se

Please follow these steps.

1. Create DB

2. Create login and password for the DB. use script below (_don't change [db\_owner]_)

3. Generate a strong password here: [https://lastpass.com/generatepassword.php](https://lastpass.com/generatepassword.php), mind that a &amp; character needs to be removed.


-----------------------------
USE [master]

GO

CREATE LOGIN [YOURDBLOGIN] WITH PASSWORD=N'GENERATEDPASSWORD', DEFAULT\_DATABASE=[YOURDBNAME], CHECK\_EXPIRATION=OFF, CHECK\_POLICY=ON

GO

USE [YOURDBNAME]

GO

CREATE USER [YOURDBLOGIN] FOR LOGIN [YOURDBLOGIN]

GO

USE [YOURDBNAME]

GO

ALTER ROLE [db\_owner] ADD MEMBER [YOURDBLOGIN]

GO

-----------------------------

3. Use created login in web.config.