
# Analysis of Transformation Capabilities Between Communication Types
This project is developed with Intellij IDEA, Angular 5 and Java 8 

## License 
This project is a part of Master Thesis from [IAAS](http://www.iaas.uni-stuttgart.de/) department of [University of Stuttgart](https://www.uni-stuttgart.de/). All the rights are reserved.
 
The source codes are published under the [Apache License v2.0](https://www.apache.org/licenses/LICENSE-2.0) as well as under the [Eclipse Public License v2.0](https://www.eclipse.org/legal/epl-v20.html) and under [Eclipse Contributor Agreement](http://www.eclipse.org/legal/ECA.php)

Copyright (c) 2018 University of Stuttgart

## Information for Users
If you only need to use the application (without extending), then do the followings.
* <b>Install</b>
    * Install [JRE 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html)
    * Install a Web application container software. (Recommended: [Tomcat 9](https://tomcat.apache.org/download-90.cgi))
    * Download the war file (transformation-analyzer.war) from the setup folder of this project or use this [link](https://github.com/mustafaakilli/master-thesis/raw/master/setup/transformation-analyzer.war)
    * Copy the war file to `{tomcat-installed-folder}\webapps\`
    * Double click to the file in `{tomcat-installed-folder}\bin\startup`
    * `{tomcat-installed-folder}` is something like `C:\Program Files\Apache Tomcat\apache-tomcat-9.0.4`
    * Type `http://localhost:8080/transformation-analyzer/` in a browser such as Chrome.
* <b>Usage</b>
    * It has three tabs. Analyze, Environments, Communications.
    * In the analyze tab:
        * Select old environment, then old communication list will be updated.
        * Selectable content will be from the supported communication list of the old communication.
        * Then select old communication and new environment fields too.
        * Click submit to see the result of the analysis.
    * In the environments tab:
        * A brand new environment can be created.
        * Or an already created environment can be edited or customized.
    * In the create communications tab:
        * A brand new communication can be created.
        * Or an already created communication can be edited or customized.
    * All the names should be UNIQUE when you are creating something! Otherwise it overwrites the existing file.
    * All the files will be saved in the local server path. `{tomcat-installed-folder}\webapps\transformation-analyzer\json_data\`
        * Please carefully read `readme.txt` before doing something there.       
        * You can edit the files.
        * Create files from the UI to avoid copy paste errors.
        * If you deploy a newer version, copy all the JSON files, created on the UI, from local server path to the desired location.
    * For the rest of the information, you can get it interactively from the UI.
    
## Information for Developers
If you want to extend or contribute this project, then do the followings.
* <b>Setting up the environment</b>
    * Install [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * Install [Intellij IDEA](https://www.jetbrains.com/idea/)
        * Use ultimate version (Free for students).
        * For community version, install Tomcat manually for Intellij IDEA.
    * Install [Node.js and npm](https://www.npmjs.com/get-npm?utm_source=house&utm_medium=homepage&utm_campaign=free%20orgs&utm_term=Install%20npm)
    * Install Angular CLI, open terminal and run `npm install -g @angular/cli`
    * Download all the project files. Go to the `{main-project-folder}\src\main\angular\`
    * Run following command to install all the required modules. `npm install`
        * This step actually is not necessary since when you build the project, it will run this command for you.
        * However it is recommended to run it here, since it takes too much time to download all modules.
        * Otherwise, building for the first time will take too much time.
    * Open the project using Intellij IDEA.
        * On the main Menu -> File -> Open -> Select the pom.xml file and open as project. 
        * It will take some time to index everything.
    * If you can't add Tomcat to Intellij IDEA, skip the next step (adding "Run Configuration").
        * Click Install from Maven Menu and WAR file will be generated. 
        * Use the same steps as given in the "User Guide" part of this document to run it.
    * Create a new "Run Configuration" in Intellij IDEA.
        * On the main menu, Run -> Edit Configurations.
        * Click green '+' button, select local.
        * Select `Tomcat 9` for Application Server.
        * In the following part, put this value `http://localhost:8080/transformation-analyzer` in the "open browser" section.
        * HTTP Port 8080 and JRE should be 1.8
        * Click "Configure", if "Angular CLI Framework detected" pop-up appears.
        * If red "fix button" appers:
            * Click the button, on the bottom-right, to add an artifact.
            * Select `transformationanalyzer:war exploded`
                * Deployment tab will be opened. Write `/transformation-analyzer` to the "Application Context".
            * Optionally, add a WAR too, if you want to have WAR output too.
                * Click green '+' button at the bottom of the window.
                * Select build artifacts, and select `transformationanalyzer:war`
                * Now you will have two output, when you build. The exploded version and war version.
        * If "fix button" does not appear:
            * Go to "Deployment" tab and click to green '+' button. 
            * Select `transformationanalyzer:war exploded` and `transformationanalyzer:war`
            * Write `/transformation-analyzer` to the "Application Context" for both artifacts.
        * Give a name and save. 
    * On the main menu click, View -> Tool Windows -> Maven Project 
    * Now Maven menu is be added to the right part of the IDE.
    * Click `Install` for building and generating targets, under "Life Cycle" of Maven Menu. 
        * It will run `npm install` and `npm build`, then it will compile java codes.
        * `Compile` from the Maven menu is not enough, thus please use `Install`.
    * After that, target folder will be generated `{main-project-folder}\target`
    * After seeing "build is successful", you can run.
    * No need to deploy, just run and it will open a browser with the url `http://localhost:8080/transformation-analyzer`
        * When you click to run, Intellij IDEA will deploy to its local server for you.
    
* <b>Project Layout and important files</b>
    * The source codes are in the folder `{main-project-folder}\src\main`
    * `{main-project-folder}\src\main\angular` is for Angular part (front-end).
        * This has three components. Analyze-Transformation, Create-Environment, Create-Communication.
        * For each tab on the UI, there is one component.
        * `rest.service.ts` makes actual requests to the Java part (servlet) to get or save content.
        * `rest.proxy.ts` is a bridge between `rest.service` and `components`.
        * `rest.proxy` will load everything just once, when the application is started. 
        * When a new file is created, it will just download that file (Other has already downloaded).
        * Components will use `rest.proxy` for all the information and there will be no need for further `GET` operation when components need information. 
            * All the communications and environments are brought to local memory from files.
            * Then all the components will be initialized.
            * All the fields and settings of the DropDown lists will be initialized in the components.
            * See [Angular2 MultiSelect Dropdown](http://cuppalabs.github.io/components/multiselectDropdown/) for details.
            * There you can find example settings and demos. 
    * `{main-project-folder}\src\main\java` is for Java part (back-end)
        * This has two main java files. RestServlet and JsonReaderWriter.
        * RestServlet handles all the `GET` and `POST` requests from the front-end.
            * Sends the contents of the files when there is `GET` request.
            * Saves the contents of the `POST` requests into the files.
            * Makes the analysis and returns the results to the front-end.
        * JsonReaderWriter handles all the conversion between Java Objects, JSON Strings and JSON files.
            * Each these three types can be converted to each other. 
            * String <=> File conversions will be used when there is save or load content.
                * String content will be retrieved from front-end and written into file.
                * File content will be converted to String and returned to front-end. 
            * Object <=> File conversions will be used when the analysis is needed.
                * The names of the files will come from front-end and the file content will be converted to Java Class Objects.
                * It is way easier to work with Java Class objects than Java JSON objects. That's why it is created.    
            * Object <=> File conversions are currently not used, however it is already implemented in JsonReaderWriter.
    * `{main-project-folder}\src\main\webapp` contains followings:
      * `json_data` has the default JSON files.
        * Please make sure that you read `readme.txt` of this folder.
        * Important warning! 
            * The newly created JSON files may be deleted if you update and redeploy the application.
            * To be safe copy all the JSON files to your local file system before redeploying.
            * Default JSONs will be preserved for sure.
      * `angular_dist` has the output of the build and compilation of angular source codes.
      * `WEB-INF` has `web.xml` where you can configure your WebApp.
        * Welcoming file is set as `angular_dist\index.html`
            * That means when you type `http://localhost:8080/transformation-analyzer/` in a browser, the content of the `angular_dist\index.html` file will be loaded.
            * So basically `http://localhost:8080/transformation-analyzer/` and `http://localhost:8080/transformation-analyzer/angular_dist/index.html` are the same.
            * `angular_dist\index.html` has all the references for the compiled scripts. It has `base href="/transformation-analyzer/angular_dist/"`
                * This is set from `pom.xml`
            * Note that you will see `base href="/"`in the source code folder, `{main-project-folder}\src\main\angular\src\index.html`
                * You don't have to edit anything there. Correct information will be written when compiling.
        * `http://localhost:8080/transformation-analyzer/index.html(index.jsp)` has no content.
      
 

## Further improvements
* Adding more communication protocol such as industrial communication protocol.
* Check the uniqueness of the names and prevent adding it.
* In case files are not loaded, retry 5-10 times and notify user.
* Detailed exception and error handling. 
* More flexible folder names and deducting them automatically (now hard-coded at the beginning of the codes).
* Delete unwanted files from interface (now manual).
* Clear OutputWindow by using a button on the UI (now refreshing page will clear).
* Move all the constant values to the `Constants.java`
* Use CSS instead of In-line HTML.
* Add comments to HTML and CSS files.
* Installing latest versions of NPM and Angular using POM.xml (now it is assumed developer has already installed them).
* Automatically copying added or generated JSON files to local file system from local server (now manual).
    - Option 1: Use post-deploy scripts (PowerShell, Bash, etc.) or ..
    - Option 2: Download button on the interface.
