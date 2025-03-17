---
parser: v2
auto_validation: true
time: 25
tags: [ tutorial>beginner, software-product-function>sap-cloud-application-programming-model]
primary_tag: software-product>sap-business-application-studio
author_name: Paola Laufer
author_profile: https://github.com/Paolaufer
---

# Develop a CAP JAVA App Using SAP Business Application Studio
<!-- description --> Develop a simple CAP Java application using SAP Business Application Studio.

## Prerequisites
- You have access to SAP Business Application Studio (see [Set Up SAP Business Application Studio for Development](appstudio-onboarding)).
- You have created a **Full-Stack Application Using Productivity Tools** dev space as described in [Create a Dev Space for Business Applications](appstudio-devspace-create).



## You will learn
- How to create a CAP project
- How to develop business applications based on the SAP Cloud Programming Model (CAP)
- How to run and test your application using the Run Configurations tool

  The application you'll develop is a simple bookshop app consisting of a data model with three entities:

  - Books
  - Authors
  - Genres

  The data model is exposed via the Catalog Service. The application has some initial data that is used for testing the application, and some custom logic that runs after reading the books from the `Books` entity.

  Once you have all the code in place, you will test the application locally.

---

### Create new CAP Java project


1. From the SAP Business Application Studio hamburger menu, select **File > New Project from Template**.

    >You can also go to the Command Palette and choose **SAP Business Application Studio: New Project From Template**.

2. Select the **CAP Project** template, and click **Start**.

    <!-- border -->![start project](1-2.png)

3. Enter **`bookshop`** as the name for the project.

4. From the **Select your runtime** dropdown list, select **Java**.

5. Select the **SAP HANA Cloud** checkbox. 

    <!-- border -->![hana](1-1.png)

6. Click **Finish**.

    The project is generated.

7. From the SAP Business Application Studio hamburger menu, select **File > Open Folder**.

8. In the command palette, open the projects folder.
    
    <!-- border -->![storyboard](1-12.png)

9. Select your project and click **OK**.

    <!-- border -->![storyboard](1-11.png)

    The Storyboard is displayed.

    <!-- border -->![storyboard](1-5.png)


### Define bookshop data schema

1. In the **Data Models** tile, click **+** to create a data model entity.

    <!-- border -->![data model](2-1.png)

2. Click **Create**.

    <!-- border -->![create entity](2-2.png)

    > It may take a few moments for the Graphical Modeler to be populated.

3. Change the entity name to **Books**.

    <!-- border -->![entity name](2-3.png)

4. Click on the entity and then click on the ![show details](2-10d.png) (Show Details) icon.

    <!-- border -->![show details](2-4.png)

5. In the **Properties** pane, click **+** to add new properties.

    <!-- border -->![show details](2-5.png)

7. Add the following properties:

    |Name| Type|
    | ---- | ---- |
    | title | String |
    | descr | String |
    | stock | Integer |
    | price | Decimal |

    The **Books** entity should look like this:

    <!-- border -->![show details](2-7.png)

8. Click **Add Entity**.

    <!-- border -->![show details](2-8.png)

9. Rename the new entity **Authors**.

10. Click on the entity and then click on the ![show details](2-10d.png) (Show Details) icon.

    <!-- border -->![show details](2-9.png)

11. In the Properties pane, click **+** to add a new property.

    <!-- border -->![show details](2-5.png)

12. Add the following property:

    |Name| Type|
    | ---- | ---- |
    | name | String | 

13. Click **Add Entity**.

    <!-- border -->![show details](2-8.png)

14. Rename the new entity **Genres**.

15. Click on the **Authors** entity and then click on the Add Relationship icon.

      <!-- border -->![show details](2-10a.png)

16. Drag the arrow to the **Books** entity.

    <!-- border -->![show details](2-11.png)

17. In the **Relationship Details** dialog, select the **To-One** radio button for the **Cardinality** and click **OK**. 
  
    <!-- border -->![show details](2-17.png)

18. Click on the **Genres** entity and then click on the Add Relationship icon.

    <!-- border -->![show details](2-18.png)

19. Drag the arrow to the **Books** entity.

    <!-- border -->![show details](2-19.png)

20. In the **Relationship Details** dialog, select the **Composition** radio button for the **Type**. 

21. Select the **To-Many** radio button for the **Cardinality** and click **OK**.
  
    <!-- border -->![show details](2-20.png)

### Define the bookshop service

1. In the storyboard, go to the **Service** tile, click **+** to create a service entity.

2. Click **Create**.

    <!-- border -->![create entity](3-1.png) 

3. Click on the service and select **Add Service Entity**.

    <!-- border -->![create entity](3-2.png) 

4. From the **Projection** dropdown list, select **Bookshop.Books**.

5. Click ![create entity](3-3.png) to save your changes.

    <!-- border -->![create entity](3-4.png) 

6. In the storyboard, click on the service and select **Add Action/Function**.

    <!-- border -->![create entity](3-5.png) 
 
7. Change the name of the action to **submitOrder**. 

    <!-- border -->![create entity](3-7.png) 

8. In the **Properties** pane, add the following:

    |Parameter Name| Parameter Type|
    | ---- | ---- |
    | amount | Integer |
    | books_id | Integer |

    <!-- border -->![create entity](3-8a.png)     


### Add sample data

1. In the storyboard, go to the **Data Models** tile, click on **Authors**, and select **Add Data**.

    <!-- border -->![create entity](4-1a.png) 

2. In the Data Editor, enter **3** for the number of rows with mock data and click **Add**.

    <!-- border -->![create entity](4-2.png) 

3. Repeat the procedure to add rows for the **Genres** and the **Books** entities.

4. In the command palette, search for the `application.yaml` file.

   <!-- border -->![create entity](4-4.png)

5. Add the following line in the `cds` section for the default profile.
   
    ```Java
    data-source.csv.paths : "test/data/**"

    ```

6. Save your changes.
   
    The file should look like this:

    <!-- border -->![create entity](4-5.png)

    This configuration tells the application where the sample data is located within the project structure.


### Create a Java class for the custom event handler

1. Go to the Explorer.
   
2. Navigate to `srv` > `src` > `main` > `java / customer / bookshop`, and create a new folder called `handlers`.

    <!-- border -->![package for Custom Event Handlers](5-2a.png)

3. Add a new file in the `handlers` folder called `BookshopServiceHandler.java`.

    <!-- border -->![package for Custom Event Handlers](5-3b.png)
   
4. Populate the `BookshopServiceHandler.java` file with the following: 

    ```Java
    package customer.bookshop.handlers;
    import org.springframework.stereotype.Component;

    import com.sap.cds.Result;
    import com.sap.cds.services.cds.CdsReadEventContext;
    import com.sap.cds.services.cds.CqnService;
    import com.sap.cds.services.handler.EventHandler;

    import com.sap.cds.services.handler.annotations.After;
    import com.sap.cds.services.handler.annotations.ServiceName;

    @Component
    @ServiceName("bookshopService")
    public class BookshopServiceHandler implements EventHandler {

    @After(event = CqnService.EVENT_READ, entity = "bookshopService.Books")
    public void onRead(CdsReadEventContext context){
    Result result = context.getResult();
    //result.forEach(r -> System.out.println(r.get("title")));
    result.forEach(r -> {
    if( ((Integer) r.get("stock"))> 111)
    r.put("title",((String)r.get("title")).concat(" -- Discount"));
    });
    }
    }

    ```

    To learn more about working with event handlers, see [Event Handlers](https://cap.cloud.sap/docs/java/event-handlers/).

5. Save your changes.
   
    Your application should look similar to the structure shown in the picture below.

    <!-- border -->![Project structure](5-4.png)

    You can also see the semantic structure of the application in the Project Overview.

    <!-- border -->![Project structure](5-4b.png)




### Test the app with local database


You can explicitly deploy your application to a persistent local `SQLite` database, or you can run your application and it will implicitly use an in-memory database.

This step describes how to run the application with an in-memory database.

You will first add all required dependencies, and then create and run a run configuration.

1. Add and install all required dependencies.

    - From the **Terminal** menu, select **New Terminal**.

    - On the `bookshop` folder, run the following:

        ```NPM
        npm install

        ```

1. From the Activity pane, open the Run Configurations view.

    <!-- border -->![Open Run Configurations view](6-1.png)

2. Click **+** at the top of the view to add a new configuration.

    <!-- border -->![Open Run Configurations view](create-new-config.png)

3. Select `Bookshop - (CAP Java)` as the runnable application from the command palette prompt.

    <!-- border -->![Open Run Configurations view](6-2.png)

    >There might be other run configuration options available in the command palette.

4. Press `Enter` to use the default name for the configuration. A new configuration is added to the run configuration tree.

5. In the **Configuration** editor, select the **Default Profile** radio button for the database type. 

    <!-- border -->![Open Run Configurations view](6-6a.png)

6. Click the green arrow on the right of the configuration name to run the application.
 
    <!-- border -->![Run](6-3.png)

7. If prompted, open the application in a new tab.
   
    <!-- border -->![Run](6-8.png)

    The application opens in the browser. 
    
8.  Click on **Books** to see the metadata and entities for the service.

    <!-- border -->![Open Run Configurations view](6-4.png)

    You can also debug your application to check your code logic. For example, to debug the custom logic for this application, perform the following steps:

9.  Place a breakpoint in the function in the `service.js` file.

10. In the running app, click the `Books` entity. It should stop at the breakpoint.

    <!-- border -->![Breakpoint](6-6.png)

11.  Click Continue in the debugger until all the books are read and the page is presented.

    <!-- border -->![Stop the app](6-7.png)

12. Remove the breakpoint.

13. Stop the application by clicking Stop in the Debugger. The number beside the Debug icon represents the number of running processes. Click Stop until there are no processes running.

   
### Deploying your app

#### Prerequisites
 - Make sure you have an SAP HANA database available in your space. See [Create an SAP HANA Database Instance Using SAP HANA Cloud Central](https://help.sap.com/docs/hana-cloud/sap-hana-cloud-administration-guide/create-sap-hana-database-instance-using-sap-hana-cloud-central)
  
 - You have an SAP HANA service (SAP HANA as a Service or SAP HANA Cloud) available in your space.
    To create a new instance in your trial account:
    1. Go to your space in the SAP BTP cockpit.
    2. Go to **Service Marketplace**.
    3. Select **SAP HANA Schemas & HDI Containers**.
    4. Click **Create**.<br>
        <!-- border -->![Create instance](create-new-instance-2.png)
    5. From the **Plan** dropdown menu, select `hdi-shared` as the service plan and provide a name for the new instance.
    6. Click **Create**.

1. From the Activity pane, open the Run Configurations view.

    <!-- border -->![Open Run Configurations view](6-1.png)

2. Click **+** at the top of the view to add a new configuration.

    <!-- border -->![Open Run Configurations view](create-new-config.png)

3. Select `Bookshop - (CAP Java)` as the runnable application from the command palette prompt.

    <!-- border -->![Open Run Configurations view](6-2.png)

    >There might be other run configuration options available in the command palette.

4. Press `Enter` to use the default name for the configuration. A new configuration is added to the run configuration tree.

5. In the **Configuration** editor, select the **SAP HANA Cloud** radio button for the database type. 

    <!-- border -->![Open Run Configurations view](6-10.png)
 
6. Log in to Cloud Foundry.

7. From the **SAP Cloud Instance** dropdown list, select the relevant instance.

8. From the **Deploy the data model before running** dropdown list, select the **Deploy (with data)** radio button. 

    <!-- border -->![Open Run Configurations view](7-12.png)


9. Click the green arrow on the right of the run configuration.

    <!-- border -->![Open Run Configurations view](6-9.png)

7. If prompted, open the application in a new tab.
   
    <!-- border -->![Run](6-8.png)

    The application opens in the browser. 

10.	From the terminal on the bookshop folder, run `cds add mta`. This adds an `mta.yaml` file to the root of your application.

    Note: If you are working on a trial account, open the `mta.yaml` file, and in the `resources` section change the `service` parameter to `hana`. Save your changes.

11.	Right-click the `mta.yaml` file and choose **Build MTA Project**.

    <!-- border -->![Build MTA](7-11.png)

      A new folder for `mta_archives` is created containing the new `mtar` file.

12. Right-click the `mtar` file and choose **Deploy MTA Archive**.

    <!-- border -->![Deploy MTA](7-12.png)

Once the task is complete, your application should be available in your Cloud Foundry space.
To access your application, go to your space in the SAP Cloud Platform cockpit and select **Applications** from the side menu.

---
