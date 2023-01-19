# Setting up our DataSource with the `lb4` CLI

So far, our application only has one endpoint, and it doesn't even connect to a datasource. To make our REST API actually functional, we need to tell it what datasource we want to use, and how it can connect to that datasource. For us IBM i developers, that datasource will mostly likely be Db2 for i.

To create a datasource, we will again use the `lb4` CLI. When we run the command to create a datasource, it will ask us what type of connector we want to use, and then download it for us. For IBM i, this connector will use ODBC as its mechanism for connecting to the system.

As you might have guessed, all we have to run is:

```bash
lb4 datasource
```

Just like when we created the application, the CLI will walk us through all of the options that we need to add to create our datasource:

```
? Datasource name: 
```
This can be anything we want, but since we are just going to be connecting to the local Db2 database, I think "Local" is a sensible name.

```
? Select the connector for Local:  (Use arrow keys)
```
![The connector prompt when creating a Datasource](assets/e.datasourceconnector.png)
Again, we are given a selection prompt that we can navigate around in. We need to press the arrow keys to go up and down to find the connector that we want to use for connecting to our data source. I'll give you one guess which one we want...

`IBM i (Db2 for i)`!

Once you select that datasource, it will continue to prompt you for options specific to our data source. Because we are using ODBC, it will ask questions about setting up an ODBC connection:

```
? ODBC DSN:
```
In this prompt, provide nothing. It would be great if we could provide `*LOCAL`, but there is a bug in the connector that only accepts values from the next question.

```
? ODBC connection string for overriding other options (e.g.: DBQ=QGPL;NAM=1):
```

For our connection string, we can provide the value `DSN=*LOCAL;`. This will make your DataSource connection use a system-wide DSN that uses `*CURRENT` credentials, meaning the REST API will run under the same authority as the user profile that is running the process (in this case, our LoopBack application).

Once you hit "Enter", you will prompted for additional connection string options that you can add. For all of these, you can just press "Enter" to leave them blank. Our DSN will do all of the heavy lifting for us, and we don't need any additional options for now.

``` 
? IBM i User (If not specified in DSN or connection string): 
? IBM i User's Password (If not specified in DSN or connection string): [hidden]
```

Just like when we created our app, the CLI will generate a file for us. However, there is one more step that we have to do before our datasource can be used: We need to compile our program.

```bash
$ npm run build
```

We now have the framework for a LoopBack application, and a datasource that allows our LoopBack application to connect to Db2 for i. We can finally start creating our models and (eventually) migrating them to the database!

---
Next: [Creating Models](f.loopback-models.md)