# Run SQL Server Linux container images with Docker

[Quickstart](https://learn.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker?view=sql-server-ver17&tabs=cli&pivots=cs1-powershell)

Prerequisites

1. Docker Desktop
2. Visual Studio + SQL Server Management Studio
3. .NET 10

## Run the container

```sh title="PowerShell"
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=P@ssw0rd1234" `
   -p 1433:1433 --name mssql-server --hostname mssql-server `
   -v C:\mssql_data:/var/opt/mssql `
   -d `
   mcr.microsoft.com/mssql/server:2025-latest
```

## Change the system administrator password

```sh title="PowerShell"
docker exec -it mssql-server /opt/mssql-tools18/bin/sqlcmd -S localhost -U sa -P "<password>" -Q "ALTER LOGIN sa WITH PASSWORD='<new-password>'"
```

## Disable the SA account as a best practice

## View list of containers

```sh title="PowerShell"
docker ps
docker logs mssql-server
```

## Connect to SQL Server

```sh title="PowerShell"
docker exec -it mssql-server "bash"
```

```bash title="BASH"
/opt/mssql-tools18/bin/sqlcmd -S localhost -U sa -P "P@ssw0rd1234" -Q "SELECT @@VERSION" -C
```

Create a new database

```sql title="SQL"
/opt/mssql-tools18/bin/sqlcmd -S localhost -U sa -P "P@ssw0rd1234" -C

CREATE DATABASE TestDB COLLATE Thai_CI_AS;

SELECT name
FROM sys.databases;
GO
```

Insert data

```sql title="SQL"
USE TestDB;

CREATE TABLE Inventory
(
    id INT,
    name NVARCHAR (50),
    quantity INT
);

INSERT INTO Inventory
VALUES (1, 'banana', 150);

INSERT INTO Inventory
VALUES (2, 'orange', 154);
GO
```

Select data

```sql title="SQL"
SELECT *
FROM Inventory
WHERE quantity > 152;
GO
```

Exit the mssql-server command prompt

```sql title="SQL"
QUIT
```

## Connect SQL Server Management Studio

Database Engine

- Server Name: 127.0.0.1
- Authentication: SQL Server Authentication
- User Name: sa
- Password:
- Database Name: <default> / master
- Encrypt: Mandatory
  - [/] Trust Server Certificate
