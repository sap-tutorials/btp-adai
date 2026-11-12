---
parser: v2
author_name: Johannes Doberer
author_profile: https://github.com/JohannesDoberer
auto_validation: true
time: 5
tags: [ tutorial>beginner, programming-tool>javascript]
primary_tag: topic>user-interface
---

# Build Skeleton React and UI5 Projects
<!-- description --> Create skeleton React and UI5 projects and install Luigi.

## Prerequisites
 - It's recommended to try the simpler examples on [GitHub](https://github.com/luigi-project/luigi/tree/master/core/examples) or the [Getting Started page](https://docs.luigi-project.io/docs/getting-started/?section=examples) before this tutorial.
 - Install [Node.js](https://nodejs.org/en/download/) 24.15.0 or newer in the Node.js 24 LTS line. This tutorial uses React 19, React Router 8 and UI5 Web Components React 2. Updating npm alone does not update Node.js.
 - You need to install [SAP Fonts](https://experience.sap.com/fiori-design-web/downloads/#sap-icon-font).

## You will learn
  - How to create a skeleton React project
  - How to create a skeleton UI5 project
  - How to add Luigi and other dependencies to your project

---

### Create React app


In this step, you will create a React skeleton project which will be used to create your Luigi app.

1. Open a Terminal or Command Prompt window and navigate to the space where you want to install the app. Then create a new folder:

    ```Shell
    mkdir luigi-react-ui5
    cd luigi-react-ui5
    ```

2. Create a new folder to host the React core app:

    ```Shell
    mkdir react-core-mf
    cd react-core-mf
    ```

3. Download the React example `package.json` from the fixed Luigi revision below. Keep `LUIGI_EXAMPLE_BASE` set in this terminal for the subsequent downloads. Install the versions used by this tutorial rather than the latest packages:

    ```Shell
    LUIGI_EXAMPLE_BASE="https://raw.githubusercontent.com/SAP/luigi/e0dcf1ad945c3c31e90d61b45860dca47a3703d7/core/examples/luigi-example-react"
    curl --fail --location "$LUIGI_EXAMPLE_BASE/package.json" --output package.json
    npm install --save-exact @luigi-project/client@2.33.0 @luigi-project/core@2.33.0 fundamental-styles@0.42.0 @ui5/webcomponents-react@2.28.2 @ui5/webcomponents-fiori@2.28.1
    npm install --save-dev --save-exact style-loader@4.0.0 css-loader@7.1.5 terser-webpack-plugin@5.6.1
    ```

    The downloaded manifest specifies React and React DOM 19.2.7 and `react-router` 8.3.0. Import router components from `react-router`, not `react-router-dom`. Keep the generated `package-lock.json` with your project and use `npm ci` for subsequent clean installations. The fixed revision also prevents changes on Luigi's `main` branch from changing the starting files.
    ​
4. The React application will be bundled using Webpack. Download the Luigi Webpack configuration from an existing [Luigi example app](https://github.com/luigi-project/luigi/blob/main/core/examples/luigi-example-react/webpack.config.js) using the line below:

    ```Shell
    curl --fail --location "$LUIGI_EXAMPLE_BASE/webpack.config.js" --output webpack.config.js
    ```

5. Next, create the project structure for the React app. Create the following folders:

    ```Shell
    mkdir -p src/views
    mkdir -p src/assets
    mkdir -p public
    ```
    ​
6. Download the Luigi React app files:

    ```Shell
    curl --fail --location "$LUIGI_EXAMPLE_BASE/public/luigi-config.js" --output public/luigi-config.js
    curl --fail --location "$LUIGI_EXAMPLE_BASE/src/index.js" --output src/index.js
    curl --fail --location "$LUIGI_EXAMPLE_BASE/src/views/home.js" --output src/views/home.js
    curl --fail --location "$LUIGI_EXAMPLE_BASE/src/views/sample1.js" --output src/views/sample1.js
    curl --fail --location "$LUIGI_EXAMPLE_BASE/src/views/sample1.js" --output src/views/sample2.js
    curl --fail --location "$LUIGI_EXAMPLE_BASE/public/index.html" --output public/index.html
    curl --fail --location "$LUIGI_EXAMPLE_BASE/public/sampleapp.html" --output public/sampleapp.html
    ```

7.  Ins​tall and run the configuration:

    ```Shell
    npm i
    npm run start
    ```
    ​
8. Stop the development server with `Ctrl+C`, then move back into the root directory:

    ```Shell
    cd ..
    ```


### Create UI5 micro-frontend


In this step, you will create a skeleton UI5 project for your UI5 micro-frontend.

1. If you didn't already, navigate to the root folder of your project in the terminal.

2. Install the UI5 project generator:

    ```Shell
    npm install -g yo generator-easy-ui5
    ```

3. Enter this command in the Terminal/Command Prompt:

    ```Shell
    yo easy-ui5 app
    ```

4. Answer the generator prompts as follows:

    - **Enter your application id (namespace)?** Enter `luigi.ui5`. This value matters: the later tutorial steps rely on it for the resource root (`luigi.ui5`) and controller names (for example `luigi.ui5.controller.Main`).
    - **Which framework do you want to use?** Choose the default (`OpenUI5`) and press Enter.
    - For all remaining questions (framework version, author, etc.), choose the default option and press Enter.

5. The generator creates the app in a folder named after the namespace (`luigi.ui5`). To match the paths used in the following tutorials, rename this folder to `ui5-mf`:

    ```Shell
    mv luigi.ui5 ui5-mf
    ```



### Tutorial navigation

**Back:** [Introduction to Luigi ←](luigi-getting-started)  |  **Next:** [Build Luigi App with React →](luigi-app-react)



---
