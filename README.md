# sama-atf
This project is an automation framework for functional testing of BPS SAMA:

- SAMA core flows
- BPS-IPS Bridge

## Content

1. [How to Set Up the framework](#setup)
2. [How to run tests](#howToRun)
    1. [From IDE](#ide)
        1. [System Integration Tests](#sit)
        2. [Test evidences](#test-evidences)
    2. [From Jenkins](#jenkins)
3. [Report Portal](#report-portal)   
4. [BPS-IPS Bridge Diagrams](#bridge)


## How to Set Up the framework <a name="setup"></a>
> **Prerequisites**
> <details>
> <summary>Install JDK and setup JAVA_HOME</summary>
> 
> 1. Download [Java SE Development Kit](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html?printOnly=1)
> 2. Click on the radio button next to Accept License Agreement
> 3. Download the 64-bit installer <br/>
> 4. Run the installer and follow the wizard steps
> 5. Setup JAVA_HOME. Click on the search button. Then type **environment**
> 6. Click on the **Edit environment variables for your account** <br/>
> ![edit variables](https://downlinko.com/assets/images/posts/development/windows-search-env.png "edit variables")
> 7. Click on **New...** <br/>
> ![environment variables](https://downlinko.com/assets/images/posts/development/windows-account-environment-variables-new.png "environment variables")
> 8. Enter **JAVA_HOME** as variable name. Enter the java installation directory as variable value. Click **OK**. <br/>
> ![JAVA_HOME var](https://downlinko.com/assets/images/posts/development/jdk/jdk-8-home-variable.png "JAVA_HOME var")
> 9. Configure the PATH environment variable. Select the **Path** variable. Click on **Edit...** <br/>
> ![edit path](https://downlinko.com/assets/images/posts/development/jdk/jdk-8-edit-path-variable.png "edit path")
> 10. Click on **New** and type **%JAVA_HOME%\bin** as shown below. Click **OK**. <br/>
> ![new env variable](https://downlinko.com/assets/images/posts/development/jdk/jdk-edit-path-variable-add-java-home.png "new env variable")
> 
> **Verification**
> 1. Click on the search button. Then type **cmd**
> 2. Click on the Command Prompt shortcut. <br/>
> ![windows cmd](https://downlinko.com/assets/images/posts/development/windows-search-cmd.png "windows cmd")
> 3. Type **java -version** and press ENTER. The above command prints the installed JDK version: 
>
>   ```
>   C:\Users\janedoe>java -version
>   java version "1.8.0_211"
>   Java(TM) SE Runtime Environment (build 1.8.0_211-b12)
>   Java HotSpot(TM) 64-Bit Server VM (build 25.211-b12, mixed mode)
>   ```
>
> The JDK is successfully installed!
> 
> </details>
> 
> <details>
> <summary>Setup Maven</summary>
> 
> 1. Download maven zip file from [official site](http://maven.apache.org/download.cgi) <br/>
> 2. Unzip it to a folder. Ex: **_C:\maven_** <br/>
> 3. Add MAVEN_HOME system variable. Press Windows key, type **adva** and click on the **_View advanced system settings_** 
> 4. In System Properties dialog, select **Advanced** tab and click on the **Environment Variables...** button.
> 5. In "Environment variables" dialog, **_System variables_**, Click on the **New...** button and add a **MAVEN_HOME** variable and point it to **_C:\maven\apache-maven-3.6.1-bin\apache-maven-3.6.1_**.
> Click **OK**.
> 6. In system variables, find **PATH**, click on the **Edit...** button. In "Edit environment variable" dialog, click on the **New** button and add this **%MAVEN_HOME%\bin**.
> Click **OK**.
> 7. **Verification.** Start a new command prompt, type **mvn -version**. Output 
> 
>     ```Apache Maven 3.6.1 (d66c9c0b3152b2e69ee9bac180bb8fcc8e6af555; 2019-04-04T22:00:29+03:00)
>     Maven home: C:\maven\apache-maven-3.6.1-bin\apache-maven-3.6.1\bin\..
>     Java version: 1.8.0_201, vendor: Oracle Corporation, runtime: C:\Program Files\Java\jdk1.8.0_201\jre
>     Default locale: en_US, platform encoding: Cp1252
>     OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows" 
>     ```    
> 
> 8. The Apache Maven is installed successfully on Windows.
> 
> </details>
> 
> <details>
> <summary>Maven settings for sama-atf</summary>
> 
> 1. Go to settings.xml from maven/conf folder (where is configured in IDE)
> 2. Add server configuration
>     ```<server>
>     <id>github</id>
>     <username>YOUR_GITHUB_USERNAME</username>
>     <password>YOUR_TOKEN</password>
>     </server>
>     ```
>
> Personal token can be created in: Settings/Developer Settings / Personal access tokens on GITHUB
> 
> </details>

**_In order to setup the the framework follow the steps:_**
1. **Install intellij IDEA**
    1. download [IntelliJ IDEA installer](https://www.jetbrains.com/idea/download/#section=windows)
    2. run the installer and follow the wizard steps
    3. select the checkbox at last step: _Run the IntelliJ IDEA_

2. **Clone the sama-atf repository** using one of the two ways possible
   <br/>HTTPS: `https://github.com/vocalink/sama-atf.git`

   <details>
   <summary>From IDE</summary>
   
      1. Go to **VCS -> Git -> Clone...**
      2. Enter the **URL** and click Test. Wait until **Connection successful** message appears
      3. Click **Clone**.
      4. When the repository is cloned the IDE will ask you to open the project file **pom.xml**. Click **Yes**.
      5. It will ask you to choose to open the project in **This Window** or **New Window**. Choose whatever is convenient to you. 
      6. IDE will load the project.
       
   </details>

   <details>
   <summary>Via command line</summary>
   
   1. Click on the search button. Then type **cmd**
   2. Click on the **Command Prompt** shortcut. <br/>
   ![search command prompt](https://downlinko.com/assets/images/posts/development/windows-search-cmd.png)
   3. Create directory by typing: **mkdir atf**
   4. Browse the folder by typing: **cd atf**
   5. Clone repository by typing: **git clone https://github.com/vocalink/sama-atf.git**
   Repository cloned.
   6. Type **mvn clean install** to download all dependencies and build the project.
   Build Success.
   7. Open IDE. Go to **File -> Open...**
   8. Select **pom.xml** file from sama-atf directory.
   9. Choose **Open as Project** option.
   IDE will open the project.
   
   </details>


## How to run tests <a name="howToRun"></a>

ATF is easy maintainable and configurable. By passing the environment short name id, the tests will run on any AWS environment. Examples:
sama-sit5, sama-sit6, sama-sit7.

### From IDE <a name="ide"></a>
   
#### System Integration Tests <a name="sit"></a>
1. Open **Run/Debug Configurations** and add a **New JUnit Configuration** with following parameters then click **OK**. <br/>

| Class:   | com.endava.bps.CucumberRunner  |
| ------------ | ------------ |
| VM options:   | **-Dspring.profiles.active=aws**<br/>Other active profiles used are: mastercard, cert <br> *Mandatory.*<br/><br/> **-DEnvName=sama-sit1**<br/> Environment for automation testing: sama-sit1, sama-sit5, sama-sit6<br> *Mandatory.*<br/><br/> **-Djenkins.run=false**<br/>For the tests run from IDE it always should be false.<br> *Mandatory.* <br/><br/> **-Drun.prerequisite.scripts=OFF**<br/> Enables some functionalities and changes configurations in order to run tests.<br/>Available options: OFF - does not run anything,<br> ON - executes changes. Adviced to use once after a new deploy. <br> *Mandatory.*<br><br/> -**Drun.over.weekend=true** <br/> Used when testing during holidays or weekend. Other option is: false <br/>*Optional.* |
| Use classpath of module:   | bps-atf-tests  |

2. Go to **sama-atf/src/test/resources/features** package and open one of existing feature files. (ex. [TF104307] Credit Transfer Flow BIRO - Happy path.feature)
3. Add **@Run** tag:
    -	**before Feature** in order to run all Scenarios from feature file <br/>
    ```
    @SIT @CLEAN @TF104307 @Run
    Feature: TF104307 - BIRO Credit Transfer Flow - Happy Path
    ```
    -	**before Scenario** in order to run only selected Scenario <br/>
    ```
    @TC2451124 @TC2448823 @TC2448831 @TC2451125 @Run
      Scenario: Check that grouped pre settlement pacs002 is generated for multiple batches
        And there is a default PACS008 message with:
          | domain      | number |
          | FILE        | 1      |
    ```
    -	**before Examples** in order to run only selected example from Scenario <br>
    ```
    @Run
     Examples:
           | TestCase  | previous_cycle | current_cycle | errorCode |
           | TC2271662 | 08             | 09            | 63        |
           | TC2271672 | 07             | 08            | 63        |
    ```
4. Make sure that the **Background** at the beginning of the SIT feature file is active, not commented:

    ```
       Background:
         Given execute aqTrigger 'FIRST_PROCESSING_DAY_END' for session number '01'
    ``` 
    
    Otherwise, RJCT pacs002 will be generated.
5. Run test by clicking on **play** button or press **Shift + F10**


#### Test evidences <a name="test-evidences"></a>
All evidences after test execution will be located in **sama-atf/target/logs** folder.

The evidence is presented in separate folders (so for every test will be created a separate folder with evidences). The name of folder is corresponding to the name of Scenario that has been ran.

The evidence folder contains: <br/>
    - **downloaded_reports** folder - contains generated reports during the test <br/>
    - **generated_ack** folder - contains received acknowledgements during the test <br/>
    - **generated_balance** folder - contains received outputs during the test <br/>
    - **generated_xml** - contains original sent NACHAM files during the test <br/>
    - **log_files** folder - contains log files with alerts generated during the test
       
### From Jenkins <a name="jenkins"></a>

1. Access [BPS Jenkins](https://jenkins.bps-votec.io/job/sama/job/sama-atf/)
2. Sign in to **GitHub** to continue to **BPS Jenkins**
3. Click on **sama/sama-atf/sama-sit-e2e-regression**
4. Click on **Build with Parameters** from the menu on the left
5. Select the **environment** and click on **Build**<br/>
   Also prerequisite/postrequisite scripts can be OFF or ON. See the step 1 in [How to run System Integration Tests from IDE](#sit) for more details.
6. Job will start the execution. Progress can be viewed in real time.
7. When job is done, cycle near the build number is either: <br/>
![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) RED, then analysis is needed, <br/>
![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) BLUE, then all tests are passed.
8. Click on **build** (ex: #41) and then click on **Cucumber Reports** link.<br/>
Test result will be displayed grouped by feature files.
9. To see the failure reason click on **failed Feature scenario** and scroll down to the **failed step**.

To see the evidence of tests click on **Workspaces** an then click on appeared link. Then go to **bps-atf-tests/target/logs**. Every test is located in separate folder. To see the evidence click on necessary folder. 

## Report Portal <a name="report-portal"></a>
Run jenkins job in order to see results on Report Portal. See [Test Evidence](#test-evidences) section steps 1-7 in [From Jenkins](#jenkins) for more details.
1. Access [Report Portal](https://report-portal.bps-votec.io/)
2. Sign in with **GitHub** to continue to **Report Portal**
3. Go to **BPS-SAMA-SIT** project
4. Click on **LAUNCHES** button from the menu on the left
5. Click on **launch** (ex: sama-sit-regression-test #23) and then click on **Root User Story**<br/>
   Test result will be displayed grouped by feature files.
![launches.png](https://user-images.githubusercontent.com/43377957/133448304-230f96c1-7e0a-48a3-9271-d187bc466c7e.png)
6. To see the failure per scenario click on failed Feature (ex: Feature: TF104307 - BIRO Credit Transfer Flow - Happy Path)
![launches.png](https://user-images.githubusercontent.com/43377957/133448448-83b184c6-0e31-4cd4-aa0b-469e86d486cf.png)
7. List of scenarios displayed. Failed scenarios marked with following Defect Type tag:<br/>
   - **Product Bug**
   - **Automation Bug**
   - **System Issue**
   - **No Defect**
   - **To investigate**
8. To see the failure reason click on failed Feature Scenario (ex: Scenario: Check that grouped pre settlement pacs002 is generated for multiple batches) and scroll down to the failed step


## BPS-IPS Bridge Diagrams <a name="bridge"></a>
**Bridge state diagram**

![img_2.png](https://user-images.githubusercontent.com/43377957/133447204-fa2e519b-b752-4667-8e5b-ae22375bf5bb.png)   

**Maps flow diagram**

![img_1.png](https://user-images.githubusercontent.com/43377957/133446948-f25b94a5-d85d-478c-909c-0eb224eedd6c.png)
