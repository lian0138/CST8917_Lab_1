
# Azure Functions Setup with HTTP Trigger, Storage Queue, and SQL Database

This guide provides setup instructions for two Azure Functions using Python 3.10 in Visual Studio Code, following the provided Microsoft tutorials. One function uses an HTTP trigger to store data in an Azure Storage Queue, and the other stores data in an Azure SQL Database. Instructions include Azure resources and local testing.

This is the YouTube link of the Lab
https://youtu.be/P1alDWdVGBA

## Azure Function with HTTP Trigger and Storage Queue Output

**Objective**: Create an HTTP trigger function that stores data in an Azure Storage Queue.

**Setup** 
- Install VS Code, Python 3.10, Azure Functions Core Tools, Func-CLI, and Azure Functions VS Code extensions.
- Create a new Azure Functions project in VS Code (Python, isolated process, HTTP trigger, Anonymous auth).
- Add queue output binding connection: `AzureWebJobsStorage`.
- In `local.settings.json`, add Storage Account connection string from Azure Portal (Storage Account → Access keys → Connection string).

**Local Testing**:
- Run `func start` in VS Code terminal to start the function host.
- Send request: `curl "http://localhost:7071/api/HttpTriggerToQueue?name=Chris.XH.Leung_946"`.
- Expected response: `Hello, Chris.XH.Leung_946! Message sent to queue.`
- Verify message in Azure Storage Explorer → Storage Account → Queues → `outqueue` (should show `Received: Chris.XH.Leung_946`).

## Azure Function with HTTP Trigger and Azure SQL Database Output

**Objective**: Create an HTTP trigger function that stores data in an Azure SQL Database.

**Azure Resources**:
- **SQL Database**: Create in Azure Portal 
  - Create a table
```
    CREATE TABLE dbo.ToDo (
    [Id] UNIQUEIDENTIFIER PRIMARY KEY,
    [order] INT NULL,
    [title] NVARCHAR(200) NOT NULL,
    [url] NVARCHAR(200) NOT NULL,
    [completed] BIT NOT NULL
	);
```
  - Allow local IP in SQL Server firewall.

**Setup**:
- Install VS Code, Python 3.10, Azure Functions Core Tools, Func-CLI, and Azure Functions VS Code extensions.
- Reuse the project with the same function trigger.
- Add SQL output binding in the  `local.settings.json` and add my password in toe the string

**Local Testing**:
- Run `func start` in VS Code terminal.
- Send POST request: `curl -X POST -H "Content-Type: application/json" -d '{"name":"Chris.XH.Leung_948"}' http://localhost:7071/api/hello`.
- Verify in Azure Portal → SQL Database → Query Editor: the latest 1000 rows in the table (should show `Name: Chris.XH.Leung_948`).