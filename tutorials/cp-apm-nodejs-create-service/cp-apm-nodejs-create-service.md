---
author_name: René Jeglinsky
author_profile: https://github.com/renejeglinsky
auto_validation: true
primary_tag: software-product-function>sap-cloud-application-programming-model
tags: [ tutorial>beginner, programming-tool>node-js, software-product>sap-business-technology-platform, software-product-function>sap-cloud-application-programming-model ]
time: 50
parser: v2
---

# Create a CAP Business Service with Node.js Using Visual Studio Code

<!-- description --> Develop a sample business service using Core Data & Services (CDS), Node.js, and SQLite, by using the SAP Cloud Application Programming Model (CAP) and developing on your local environment.

## You will learn

- How to develop a sample business service using CAP and `Node.js`
- How to define a simple data model and a service that exposes the entities you created in your data model
- How to run your service locally
- How to deploy the data model to an `SQLite` database
- How to add custom handlers to serve requests that aren't handled automatically

## Prerequisites

- You've installed [Node.js](https://nodejs.org/en/download/). Make sure you run the latest long-term support (LTS) version of Node.js with an even number like 20. Refrain from using odd versions, for which some modules with native parts will have no support and thus might even fail to install. In case of problems, see the [Troubleshooting guide](https://cap.cloud.sap/docs/advanced/troubleshooting#npm-installation) for CAP.
- You've installed the latest version of [Visual Studio Code](https://code.visualstudio.com/) (VS Code).
- (Windows only) You've installed the [SQLite](https://sqlite.org/download.html) tools for Windows. Find the steps how to install it in the [How Do I Install SQLite](https://cap.cloud.sap/docs/advanced/troubleshooting#how-do-i-install-sqlite-on-windows) section of the CAP documentation.
- You've installed an HTTP client, for example, [REST client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client).
- If you don't have a Cloud Foundry Trial subaccount and dev space on [SAP BTP](https://cockpit.hanatrial.ondemand.com/cockpit/) yet, create your [Cloud Foundry Trial Account](hcp-create-trial-account) with **US East (VA) as region** and, if necessary [Manage Entitlements](cp-trial-entitlements). You need this to continue after this tutorial.

---

## Intro

Before you start, make sure that you've completed the prerequisites.

### Set up local development environment

1. Open a command line window and install the `cds` development kit globally by executing the following command:

    ```Shell/Bash
    npm i -g @sap/cds-dk
    ```

    > This process takes some minutes installing the `cds` command, which you will use in the next steps.

    > On MacOS/Linux, you need to follow the steps as described [here](https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally).

    > If there's an older `@sap/cds` package already installed on your machine, you have to remove it first; if so, you'll be instructed to do so.

    > In case of problems, see the [Troubleshooting guide](https://cap.cloud.sap/docs/advanced/troubleshooting#npm-installation) in the CAP documentation for more details.

2. To verify that the installation was successful, run `cds` without arguments:

    ```Shell/Bash
    cds
    ```

    <!-- border; size:540px -->![cds commands](cds_commands.png)

    > This lists the available `cds` commands. For example, use `cds --version` to check the version that you've installed. To know what is the latest version, see the [Release Notes](https://cap.cloud.sap/docs/releases/) for CAP.

### Install VS Code extension

1. Go to [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=SAPSE.vscode-cds).

2. Choose **Install**.

    <!-- border; size:540px -->![extension_marketplace](VSCode_extension.png)

    > VS Code opens the extensions details page.

3. In VS Code choose **Install** to enable the extension for SAP CDS Language Support.

    <!-- border; size:540px -->![extension_VSCode](VSCode_view_extension.png)

    > If the extension is already installed and enabled in VS Code, it will be updated automatically.

    > Learn more about the features in this short [demo](https://www.youtube.com/watch?v=eY7BTzch8w0) and see the [features and commands](https://cap.cloud.sap/docs/tools/cds-editors#cds-editor) in the CAP documentation.

### Start project

[OPTION BEGIN [Windows]]

With your installed CDS command line tool, you can now create a new CAP-based project, in the form of a new directory with various things preconfigured, and run it.

1. Open a command line window and run the following command in a folder of your choice to create the project:

    ```Shell/Bash
    cds init my-bookshop
    ```

    > This creates a folder `my-bookshop` in the current directory.

2. In VS Code, go to **File** **&rarr;** **Open Folder** and choose the **`my-bookshop`** folder.

3. Go to **Terminal** **&rarr;** **New Terminal** to open a command line window within VS Code and run the following command in the root level of your project:

    ```Shell/Bash
    npm install
    ```

4. In the command line window run the following:

    ```Shell/Bash
    cds watch
    ```

    > This command tries to start a `cds` server. Whenever you feed your project with new content, for example, by adding or modifying `.cds`, `.json`, or `.js` files, the server automatically restarts to serve the new content.

    > As there's no content in the project so far, it just keeps waiting for content with a message as shown:

    ```Shell/Bash
    cds serve all --with-mocks --in-memory?
    watching: cds,csn,csv,ts,mjs,cjs,js,json,properties,edmx,xml,env,css,gif,html,jpg,png,svg...
    live reload enabled for browsers
            _______________________


        No models found in db/,srv/,app/,schema,services.
        Waiting for some to arrive...
    ```

[OPTION END]

[OPTION BEGIN [MacOS and Linux]]

1. Open a command line window and run the following command in a folder of your choice to create the project:

    ```Shell/Bash
    cds init my-bookshop
    ```

    > This creates a folder `my-bookshop` in the current directory.

2. Open VS Code, go to **File** **&rarr;** **Open** and choose the **`my-bookshop`** folder.

3. Go to **View** **&rarr;** **Command Palette** **&rarr;** **Terminal: Create New Terminal** to open a command line window within VS Code and run the following command in the root level of your project:

    ```Shell/Bash
    npm install
    ```

4. In the command line window run the following:

    ```Shell/Bash
    cds watch
    ```

    > This command tries to start a `cds` server. Whenever you feed your project with new content, for example, by adding or modifying `.cds`, `.json`, or `.js` files, the server automatically restarts to serve the new content.

    > As there's no content in the project so far, it just keeps waiting for content with a message as shown:

    ```Shell/Bash
    cds serve all --with-mocks --in-memory?
    watching: cds,csn,csv,ts,mjs,cjs,js,json,properties,edmx,xml,env,css,gif,html,jpg,png,svg...
    live reload enabled for browsers
            _______________________


        No models found in db/,srv/,app/,schema,services.
        Waiting for some to arrive...
    ```

[OPTION END]

### Define your first service

After initializing the project, you should see the following empty folders:

- `app`: for UI artifacts
- `db`: for the database level schema model
- `srv`: for the service definition layer

  <!-- border; size:540px -->![Folder structure](folder_structure.png)

1. Let's feed it by adding a simple domain model. In the **`srv`** folder choose the **New File** icon in VS Code and create a new file called `cat-service.cds`.

2. Add the following code to the file `cat-service.cds`:

    ```CDS
    using { Country, managed } from '@sap/cds/common';

    service CatalogService {

      entity Books {
        key ID : Integer;
        title  : localized String;
        author : Association to Authors;
        stock  : Integer;
      }

      entity Authors {
        key ID : Integer;
        name   : String;
        books  : Association to many Books on books.author = $self;
      }

      entity Orders : managed {
        key ID  : UUID;
        book    : Association to Books;
        country : Country;
        amount  : Integer;
      }

    }
    ```

    > Remember to save your files choosing <kbd>Ctrl</kbd> + <kbd>S</kbd>.

3. As soon as you've saved your file, the still running `cds watch` reacts immediately with some new output as shown below:

    ```Shell/Bash
    [cds] - connect using bindings from: { registry: '~/.cds-services.json' }
    [cds] - connect to db > sqlite { database: ':memory:' }
     > init from ./db/csv/my.bookshop-Authors.csv
    /> successfully deployed to sqlite in-memory db

    [cds] - serving CatalogService { at: '/odata/v4/catalog', impl: './srv/cat-service.js' }

    [cds] - server listening on { url: 'http://localhost:4004' }
    [cds] - launched at 18/05/2022, 19:49:32, in: 874.456ms
    [cds] - [ terminate with ^C ]
    ```

    > This means, `cds watch` detected the changes in `srv/cat-service.cds` and automatically bootstrapped an in-memory SQLite database when restarting the server process.

4. To test your service, go to: <http://localhost:4004>

    <!-- border; size:540px -->![Generated index page](application_local.png)

    > You won't see data, because you haven't added a data model yet. Click on the available links to see the service is running.

### Provide mock data

Add service provider logic to return mock data.

1. In the **`srv`** folder, create a new file called `cat-service.js`.

2. Add the following code to the file `cat-service.js`:

    ```JavaScript
    module.exports = (srv) => {

     // Reply mock data for Books...
     srv.on ('READ', 'Books', ()=>[
       { ID:201, title:'Wuthering Heights', author_ID:101, stock:12 },
       { ID:251, title:'The Raven', author_ID:150, stock:333 },
       { ID:252, title:'Eleonora', author_ID:150, stock:555 },
       { ID:271, title:'Catweazle', author_ID:170, stock:222 },
     ])

     // Reply mock data for Authors...
     srv.on ('READ', 'Authors', ()=>[
       { ID:101, name:'Emily Brontë' },
       { ID:150, name:'Edgar Allen Poe' },
       { ID:170, name:'Richard Carpenter' },
     ])

    }
    ```

    > Remember to save your files choosing <kbd>Ctrl</kbd> + <kbd>S</kbd>.

3. To test your service, click on these links:

    - <http://localhost:4004/odata/v4/catalog/Books>

    - <http://localhost:4004/odata/v4/catalog/Authors>

    > You should see the mock data that you've added for the `Books` and `Authors` entities.

### Add data model and adapt service definition

To get started quickly, you've already added a simplistic all-in-one service definition. However, you would usually put normalized entity definitions into a separate data model and have your services expose potentially de-normalized views on those entities.

1. In the **`db`** folder choose the **New File** icon in VS Code and create a new file called `schema.cds`.

2. Add the following code to the file `schema.cds`:

    ```CDS
    namespace my.bookshop;
    using { Country, managed } from '@sap/cds/common';

    entity Books {
      key ID : Integer;
      title  : localized String;
      author : Association to Authors;
      stock  : Integer;
    }

    entity Authors {
      key ID : Integer;
      name   : String;
      books  : Association to many Books on books.author = $self;
    }

    entity Orders : managed {
      key ID  : UUID;
      book    : Association to Books;
      country : Country;
      amount  : Integer;
    }
    ```

3. Open the file `cat-service.cds` and replace the existing code with:

    ```CDS
    using my.bookshop as my from '../db/schema';

    service CatalogService {
      entity Books @readonly as projection on my.Books;
      entity Authors @readonly as projection on my.Authors;
      entity Orders @insertonly as projection on my.Orders;
    }
    ```

    > Remember to save your files choosing <kbd>Ctrl</kbd> + <kbd>S</kbd>.

### Add initial data

You will add plain CSV files in folder `db/data` to fill your database tables with initial data. In the command line window execute the following:

```sh
cds add data
```

This adds csv files with a single header line for all entities to the `db/data/` folder. The name of the files matches the entities' namespace and name, separated by `-`.

1. Open the file `db/data/my.bookshop-Authors.csv` and add the following sample data to the file:

    ```CSV
    101,Emily Brontë
    107,Charlote Brontë
    150,Edgar Allen Poe
    170,Richard Carpenter
    ```

2. Open the file `db/data/my.bookshop-Books.csv` and add the following sample data to the file:

    ```CSV
    201,Wuthering Heights,101,12
    207,Jane Eyre,107,11
    251,The Raven,150,333
    252,Eleonora,150,555
    271,Catweazle,170,22
    ```

    > Remember to save your files choosing <kbd>Ctrl</kbd> + <kbd>S</kbd>.

    > For now, you can ignore the additional files that `cds add data` generated for you.

    > After you added these files, `cds watch` restarts the server with an output, telling that the files have been detected and their content been loaded into the database automatically:

    ```Shell/Bash
    [cds] - connect using bindings from: { registry: '~/.cds-services.json' }
    [cds] - connect to db > sqlite { database: ':memory:' }
     > init from ./db/csv/my.bookshop-Books.csv
    /> successfully deployed to sqlite in-memory db

    [cds] - serving CatalogService { at: '/odata/v4/catalog', impl: './srv/cat-service.js' }

    [cds] - server listening on { url: 'http://localhost:4004' }
    [cds] - launched at 18/05/2022, 19:49:47, in: 861.036ms
    [cds] - [ terminate with ^C ]
    ```

3. Remove the code with mock data in `cat-service.js`, because you want to see the data loaded from the `csv` files.

4. To test your service, open a web browser and go to:

    <http://localhost:4004/odata/v4/catalog/Books>

    <http://localhost:4004/odata/v4/catalog/Authors>

    > As you now have a fully capable SQL database with some initial data, you can send complex OData queries, served by the built-in generic providers.

    <http://localhost:4004/odata/v4/catalog/Authors?$expand=books($select=ID,title)>

    > You should see a book titled Jane Eyre. If not, make sure you've removed the mock data from `cat-service.js`.

### Add persistent database

Before you continue, make sure that you've completed the prerequisites and installed SQLite (for Windows users only).

Instead of using in-memory, you can also use persistent databases.

1. If `cds watch` is running, choose <kbd>Ctrl</kbd> + <kbd>C</kbd> in the command line to stop the service.

2. Configure the database in your `package.json` in the `cds.requires.db` section.

    ```json
    {
      "name": "my-bookshop",
      "version": "1.0.0",
      "description": "A simple CAP project.",
      "repository": "<Add your repository here>",
      "license": "UNLICENSED",
      "private": true,
      "dependencies": {
        "@sap/cds": "^7",
        "express": "^4"
      },
      "devDependencies": {
        "@cap-js/sqlite": "^1"
      },
      "scripts": {
        "start": "cds-serve"
      },
      "cds": { "requires": {
       "db": {
          "kind": "sqlite",
          "credentials": { "url": "db/my-bookshop.sqlite" }
       }
    }}
    }
    ```

3. Deploy the data model to an `SQLite` database:

    ```Shell/Bash
    cds deploy
    ```

    > You've now created an `SQLite` database file under `db/my-bookshop.sqlite`.

    > The difference to the automatically provided in-memory database is that you now get a persistent database stored in the local file.

4. Open `SQLite` and view the newly created database:

    ```Shell/Bash
    sqlite3 db/my-bookshop.sqlite -cmd .dump
    ```

    > If this doesn't work, check if you have [SQLite](https://sqlite.org/download.html) installed. On Windows, you might need to enter the full path to SQLite, for example: `C:\sqlite\sqlite3 db/my-bookshop.db -cmd .dump`. Find the steps how to install it in the Troubleshooting guide in section [How Do I Install SQLite](https://cap.cloud.sap/docs/advanced/troubleshooting#how-do-i-install-sqlite-on-windows) in the CAP documentation for more details.

5. To stop `SQLite` and go back to your project directory, choose <kbd>Ctrl</kbd> + <kbd>C</kbd>.

6. Run your service.

    ```Shell/Bash
    cds watch
    ```

    ```Shell/Bash
    [cds] - connect using bindings from: { registry: '~/.cds-services.json' }
    [cds] - connect to db > sqlite { url: 'sqlite.db', database: 'db/my-bookshop.db' }
    [cds] - serving CatalogService { at: '/odata/v4/catalog', impl: './srv/cat-service.js' }

    [cds] - server listening on { url: 'http://localhost:4004' }
    [cds] - launched at 18/05/2022, 19:54:53, in: 830.398ms
    [cds] - [ terminate with ^C ]
    ```

### Test generic handlers

You can now see the generic handlers shipped with CAP in action.

In the root of your project, create a file called `test.http` and copy the following requests in it. This file can be used with the [REST client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) to make requests against your service. The generic handlers CAP provides sent the responses to your requests.

```HTTP
###
#
# Browse Books
#
GET http://localhost:4004/odata/v4/catalog/Books?
  # &$select=title,stock
  # &$expand=currency
  # &sap-language=de

###
#
# Get Author wit ID 101
#
GET http://localhost:4004/odata/v4/catalog/Authors(101)

###
#
# Update Author with ID 101
#
POST http://localhost:4004/odata/v4/catalog/Authors
Content-Type: application/json

{"ID": 101, "name": "Some Author"}


###
#
# Order a Book
#
POST http://localhost:4004/odata/v4/catalog/Orders
Content-Type: application/json;IEEE754Compatible=true

{"book_ID": 201, "amount": 5}


```

Click on `Send Request` inside the `test.http` file, to execute requests against your service.

<!-- border; size:540px -->![Send a request](send_request.png)

> This `Send Request` button is provided by the [REST client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client). It appears for every single request. This is important for the following step, when you execute, for example, the `Order a Book` request.

The REST client gives you the response of your service and you see immediately if the request was successful.

Use the following request to answer the next question:

```HTTP
POST http://localhost:4004/odata/v4/catalog/Books
Content-Type: application/json

{"ID": 201, "title": "Some Title"}

```

### Add custom logic

1. In VS Code open the file `cat-service.js` and replace the existing code with:

    ```JavaScript
      module.exports = (srv) => {

      const {Books} = cds.entities ('my.bookshop')

      // Reduce stock of ordered books
      srv.before ('CREATE', 'Orders', async (req) => {
        const order = req.data
        if (!order.amount || order.amount <= 0)  return req.error (400, 'Order at least 1 book')
        const tx = cds.transaction(req)
        const affectedRows = await tx.run (
          UPDATE (Books)
            .set   ({ stock: {'-=': order.amount}})
            .where ({ stock: {'>=': order.amount},/*and*/ ID: order.book_ID})
        )
        if (affectedRows === 0)  req.error (409, "Sold out, sorry")
      })

      // Add some discount for overstocked books
      srv.after ('READ', 'Books', each => {
        if (each.stock > 111)  each.title += ' -- 11% discount!'
      })

    }
    ```

    > Remember to save your files choosing <kbd>Ctrl</kbd> + <kbd>S</kbd>.

    > Whenever orders are created, this code is triggered. It updates the book stock by the given amount, unless there aren't enough books left.

2. In the `test.http` file, execute the `Browse Books` request.

    > Look at the stock of book `201`.

    <!-- border; size:540px -->![Test the request](get_stock.png)

3. Execute the `Order a Book` request.

    > This triggers the logic above and reduces the stock.

4. Execute the `Browse Books` request again.

    > The stock of book `201` is lower than before. In the response you also see, that overstocked books get a discount now.