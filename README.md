# Migrate from Syncfusion Scheduler to Bryntum Scheduler Pro - backend starter repository

## Setup a MySQL database locally

We’ll install MySQL Server and MySQL Workbench. MySQL Workbench is a MySQL GUI that we’ll use to create a database with tables for the Scheduler data and to run SQL queries. Download MySQL Server and MySQL Workbench from the [MySQL community downloads page](https://dev.mysql.com/downloads/). If you’re using Windows, you can use the [MySQL Installer for Windows](https://dev.mysql.com/downloads/installer/) to download MySQL products. Use the default configurations when configuring MySQL Server and Workbench. Make sure that you configure the MySQL Server to start at system startup for your convenience.

Open the MySQL Workbench desktop application. Open the local instance of the MySQL Server that you configured.

We’ll write our MySQL queries in the query tab and execute the queries by pressing the yellow lightning bolt button.

### Create a MySQL database for the Syncfusion Scheduler data: Adding tables and example data

Let’s run some MySQL queries in MySQL Workbench to create, use, and populate a database for our Syncfusion Scheduler data. Execute the following query to create a database called syncfusion_scheduler:


```sql
CREATE DATABASE syncfusion_scheduler;
```

Run the following query so that we set our newly created database for use:

```sql
USE syncfusion_scheduler;
```

Create a table for the appointments data:

```sql
CREATE TABLE `appointments` (
  `Id` int(11) NOT NULL AUTO_INCREMENT,
  `Text` varchar(200) DEFAULT NULL,
  `StartTime` datetime NOT NULL,
  `EndTime` datetime NOT NULL,
  `StartTimezone` varchar(200) DEFAULT NULL,
  `EndTimezone` varchar(200) DEFAULT NULL,
  `Color` varchar(200) DEFAULT NULL,
  `Location` varchar(200) DEFAULT NULL,
  `Description` varchar(200) DEFAULT NULL,
  `Subject` varchar(200) DEFAULT NULL,
  `IsAllDay` boolean NOT NULL,
  `IsBlock` boolean,
  `Categorize` varchar(200) DEFAULT NULL,
  `ResourceId` json NOT NULL,
  `Priority` varchar(200) DEFAULT NULL,
  `RecurrenceID` int(11) DEFAULT NULL,
  `FollowingID` int(11) DEFAULT NULL,
  `ResourceFields` varchar(200) DEFAULT NULL,
  `RecurrenceRule` varchar(200) DEFAULT NULL,
  `RecurrenceExDate` varchar(200) DEFAULT NULL,
   PRIMARY KEY (`Id`)
);
```

Create a table for the resources data:

```sql
CREATE TABLE `resources` (
  `Id` int(11) NOT NULL AUTO_INCREMENT,
  `Text` varchar(200) DEFAULT NULL,
  `Color` varchar(200) DEFAULT NULL,
   PRIMARY KEY (`Id`)
);
```

Add some example appointments data to the appointments table:

```sql
INSERT INTO appointments (
    `Id`, 
    `Subject`, 
    `StartTime`, 
    `EndTime`, 
    `IsAllDay`,
    `ResourceId`,
    `Priority`,
    `RecurrenceRule`,
    `IsBlock`
) 
VALUES 
    (1, 'Meeting', '2024-04-15T10:00:00', '2024-04-15T11:00:00', false, '[1]', 'low', NULL, NULL),
    (2, 'Presentation', '2024-04-15T14:30:00', '2024-04-15T16:00:00', false, '[2]', 'high', 'FREQ=DAILY;INTERVAL=1;COUNT=5;', NULL),
    (3, 'Interview', '2024-04-15T10:00:00', '2024-04-15T11:30:00', false, '[3]', 'low', 'FREQ=DAILY;INTERVAL=4;', NULL),
    (4, 'Project proposal', '2024-04-15T15:00:00', '2024-04-15T16:30:00', false, '[4]', 'low', NULL, NULL),
    (5, 'Interview', '2024-04-15T15:50:00', '2024-04-15T17:30:00', false, '[4]', 'high', NULL, NULL),
    (6, 'Tax filing', '2024-04-15T09:30:00', '2024-04-15T10:30:00', false, '[5]', 'high', NULL, NULL),
    (7, 'Finance seminar', '2024-04-15T13:30:00', '2024-04-15T14:00:00', false, '[6]', 'low', NULL, NULL),
    (8, 'Quarterly reports', '2024-04-15T09:00:00', '2024-04-15T10:00:00', false, '[6]', 'high', NULL, NULL),
    (9, 'Performance review', '2024-04-15T10:30:00', '2024-04-15T12:00:00', false, '[3]', 'high', NULL, NULL),
    (10, 'Business conference', '2024-04-15T09:30:00', '2024-04-15T11:00:00', false, '[2]', 'low', NULL, NULL),
    (11, 'Lunch', '2024-04-15T12:00:00', '2024-04-15T13:00:00', false, '[1, 2, 3, 4, 5, 6]', NULL, 'FREQ=DAILY;INTERVAL=1;', true);
```

The last event is a block event, the `IsBlock` property is set to `true`. It prevents event creation in the time range of the block event.

Add some example resources data to the resources table:

```sql
INSERT INTO resources (
    `Text`, 
    `Color`
) 
VALUES 
    ( "Jane", "#1a237e" ),
    ( "Lisa", "#283593" ),
    ( "Edward", "#303f9f" ),
    ( "Sarah", "#3949ab" ),
    ( "James", "#3f51b5" ),
    ( "Chris", "#5c6bc0" );
```

You’ll be able to view the example appointments data by running the following query:

```sql
SELECT * FROM appointments;
```

You’ll be able to view the example resources data by running the following query:

```sql
SELECT * FROM resources;
```

## Set up the backend server

Install the dependencies by running the following command:

```bash
npm install
```

In the `utils/dbConnect.js` file, the Express server uses the [MySQL2](https://github.com/sidorares/node-mysql2) library to connect to a MySQL database. A connection pool is created using the MySQL2 `createPool` method. The connection pool is exported into the `server.js` file where it's used to run database queries for CRUD operations using the `query` method.

Now create a `.env` file in the root folder and add the following lines for connecting to the MySQL database that we’ll create:

```
HOST=localhost
PORT=1338
DB_USER=root
PASSWORD=
DATABASE=syncfusion_scheduler
FRONTEND_URL="http://localhost:4000"
```

Don’t forget to add the root password for your MySQL server.

## Running the application

Run the server locally using the following command:

```bash
npm start
```
