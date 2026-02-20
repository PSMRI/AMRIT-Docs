# For API repositories

### Building From Source

The API microservices are built on Java, Spring boot framework and MySQL Database.

#### Pre-requisites

* JDK 17 LTS
* Spring Tool Suite 3 / Eclipse / Visual Studio Code/ IntelliJ IDE
* Maven (if not pre-installed with the editor)
* Redis
* MySQL  8.0 and MySQL Workbench
* Copy `src/main/environment/<module>_example.properties` to `src/main/environment/<module>_local.properties` and edit the file accordingly.

### Command Line
To build and run your Maven project, open the CLI.
   * Navigate to your project directory using the `cd` command.
   * Use the Maven command `mvn clean install` to build your project.
   * To run your Java application, use `mvn spring-boot:run -DENV_VAR=local`. Ensure the Redis server is open during the run.
   * Once the run is complete, load `http://localhost:{PORT}/swagger-ui.html#!/`, where PORT is defined in the [AMRIT documentation](https://github.com/PSMRI/AMRIT/blob/main/README.md).


### STS/Eclipse
#### Create Build Configuration

* In your editor, click on Run -> Run configuration.
* Double-click on Maven build and give a suitable name for the new configuration.
* Populate the base directory by clicking on workspace and selecting the API module.
* Set goals to clean install -DENV\_VAR=local(your choice of the desired environment) and 
  then 
  Go to **Environment** tab and click **Add**.  Set
     ```
     Name : ENV_VAR 
     Value: local
     ```
   click on Apply.
* It is advisable to have a personal environment properties file under src/main/environment filling out all the placeholders to avoid repetitive manual work each time you run locally.
* Click Run to run the build configuration.

#### Create Run Configuration

* In your editor, click on Run -> Run configuration.
* Double-click on Spring Boot App(in STS) / Java Application(in Eclipse) and give a suitable name for the new configuration.
* Select the project and main class and click on Apply.
* Click Run to run the configuration. Keep the Redis server open during this run.
* Once the run is complete, load `http://localhost:{PORT}/swagger-ui.html#!/`, where PORT is defined in the [AMRIT documentation](https://github.com/PSMRI/AMRIT/blob/main/README.md).


### Visual Studio Code

1. **Install Visual Studio Code**: Download and install [Visual Studio Code](https://code.visualstudio.com/) from the official website.
2. **Install Java** **and** **Maven**.
3. **Install Visual Studio Code Extensions**:
   * **Java Extension Pack**: Open Visual Studio Code, go to the Extensions view by clicking on the Extensions icon in the Activity Bar, and search for "Java Extension Pack." Install it to get Java support and Maven integration.
4. **Open the Project in Visual Studio Code**:
   * Launch Visual Studio Code.
   * Use the **File > Open Folder** option to open your Maven project folder.
5. **Configure Java and Maven**:
   * If not already configured, set up your Java Home and Maven Home in Visual Studio Code.
   * Go to **File > Preferences > Settings**, search for "Java Home" and "Maven Home," and specify the paths to your JDK and Maven installations.
6. **Build and Run**:
   * To build and run your Maven project, open the integrated terminal in Visual Studio Code (**Terminal > New Terminal**).
   * Navigate to your project directory using the `cd` command.
   * Use the Maven command `mvn clean install` to build your project.
   * To run your Java application, use `mvn spring-boot:run -DENV_VAR=local`. Ensure the Redis server is open during the run.
   * Once the run is complete, load `http://localhost:{PORT}/swagger-ui.html#!/`, where PORT is defined in the [AMRIT documentation](https://github.com/PSMRI/AMRIT/blob/main/README.md).
