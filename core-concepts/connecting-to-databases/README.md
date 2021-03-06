---
description: >-
  You can connect to, and run queries to fetch and update data directly from the
  Appsmith editor.
---

# Connecting to Databases

## Supported Databases

* [PostgreSQL](../../datasource-reference/querying-postgres.md)
* [MongoDB](../../datasource-reference/querying-mongodb/)
* [Redshift](../../datasource-reference/querying-redshift.md)
* [MySQL](../../datasource-reference/querying-mysql.md)
* [Firestore](../../datasource-reference/querying-firestore.md)
* ElasticSearch
* DynamoDB
* Redis
* MS SQL

## Security

Appsmith safely encrypts all your database credentials and stores them securely. Appsmith also does not store any data returned from your data sources and acts only as a proxy layer to orchestrate the API / Query calls. Since Appsmith is an open-source framework, you can [deploy it on-premise](../../getting-started/setting-up/), and audit it to ensure none of your data leaves your VPC.

## Connecting to a Database

{% hint style="warning" %}
Before beginning, you must whitelist the IP address of the Appsmith deployment on your database instance. 

18.223.74.85 is the IP address of the Appsmith cloud instance.

Follow the step-by-step guide to [whitelist appsmith on AWS.](aws-whitelist.md)
{% endhint %}

Once the appsmith instance has been whitelisted:

1. Go to **DB Queries** on that page.
2. Click on **+** next to **DB Queries**
3. You’ll see a list of existing Datasources that this page can connect to
4. Click on + New Datasource
5. Choose a Database from the [supported list](./#supported-databases)
6. Provide the configuration details required to connect to your database. You may need to contact your database admin.
7. Click on **Test** to verify that Appsmith is able to connect to your database using the details provided by you
8. **Save** your connection

![](../../.gitbook/assets/db-connect.gif)

{% hint style="info" %}
Databases configured within one page of an app can be accessed by developers anywhere within the organization
{% endhint %}

## General Notes

Appsmith creates a new connection pool with the database server when you first connect the database server to your app. All subsequent queries executed by Appsmith against your database then re-use this connection. This ensures that at run-time your queries are executed quickly.

In the event of a server restart/update, all connections to your database server are closed. In this case, Appsmith opens a new connection when your application executes its first query. This connection is then maintained for future use. If the connection has been idle for a long time, your database server may close the connection. Again, in this case, Appsmith will open a new connection on the first query execution.

For certain plugins like PostgreSQL, and MySQL, Appsmith creates and maintains a connection pool because multiple queries cannot be executed against a single connection.

#### Concurrent queries

Appsmith limits maximum queries that can run concurrently on a database to be 5. If the application attempts to make more queries concurrently, you'll see an error saying `Connection not available`.

