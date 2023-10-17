# <p align="center">  Cucumber Setup
</p>
  
  # Table of Contents

- [Table of Contents](#table-of-contents)
- [Setting Up a Development Environment](#setting-up-a-development-environment)
  - [Prerequisites](#prerequisites)
  - [Step 1: Download IntelliJ Community Edition](#step-1-download-intellij-community-edition)
  - [STEP2: tar -xzf ideaIC-2023.2.2.tar.gz](#step2-tar--xzf-ideaic-202322targz)
  - [Step3: cd idea-IC-232.9921.47/bin/](#step3-cd-idea-ic-232992147bin)
  - [Step4: ./idea.sh](#step4-ideash)
  - [Configuring IntelliJ IDEA](#configuring-intellij-idea)
- [Java 11 installation](#java-11-installation)
  - [Step 5:sudo apt install openjdk-11-jre-headless](#step-5sudo-apt-install-openjdk-11-jre-headless)
  - [Installation of Maven](#installation-of-maven)
  - [Step6: sudo apt install maven](#step6-sudo-apt-install-maven)
  - [Creating a Maven Project with Cucumber Dependencies](#creating-a-maven-project-with-cucumber-dependencies)
  - [Plugin Section](#plugin-section)
    - [Step 9: Configure Maven Compiler Plugin](#step-9-configure-maven-compiler-plugin)
  - [Search.feature](#searchfeature)
  - [Adding Step Definitions Automatically (Auto Generation of Packages, Classes, and Methods)](#adding-step-definitions-automatically-auto-generation-of-packages-classes-and-methods)
- [Adding Step Definitions Automatically (Auto Generation of Packages, Classes, and Methods)](#adding-step-definitions-automatically-auto-generation-of-packages-classes-and-methods-1)
  - [Navigate to a Step in the Feature File](#navigate-to-a-step-in-the-feature-file)
  - [Generate Step Definitions](#generate-step-definitions)
  - [Choose "Create All Step Definitions"](#choose-create-all-step-definitions)
  - [Select Step Definitions Location](#select-step-definitions-location)
  - [Generate Step Definitions](#generate-step-definitions-1)
    - [StepDefinitions.java](#stepdefinitionsjava)
  - [Testing](#testing)

# Setting Up a Development Environment

## Prerequisites
Before you can start developing with IntelliJ IDEA, Java, and Maven, make sure you have the following prerequisites in place:

- **Internet Connection:** Ensure that you have a stable internet connection. You may need to download additional dependencies during the setup process.

- **IntelliJ IDEA (Community Edition):** Make sure you have IntelliJ IDEA installed on your system. This document is based on version 2023.1.2, but you can use the latest available version. Download IntelliJ IDEA Community Edition from [official website](https://www.jetbrains.com/idea/download).

- **Java:** You'll need Java installed on your system to develop and run Java applications. This document is based on Java version 11.0.20.1.

- **Maven:** Maven is a build and project management tool used in many Java projects. Ensure that you have Maven installed. This document is based on Maven version 3.6.3.

- **Minimal Requirements:**
  - 8 GB RAM.
  - At least 500MB of free disk space even if the IDE is already installed.
  - A supported version of a common Linux distribution. Specifically, Ubuntu 18.04 LTS, Ubuntu 20.04 LTS, Ubuntu 22.04 LTS, Ubuntu 22.10, CentOS, Debian, and RHEL are supported.

## Step 1: 

Download IntelliJ Community Edition

![Alt text](1.png)

1. First, download the IntelliJ Community Edition (An open-source IDE) from the [official website](https://www.jetbrains.com/idea/download).


Once you have downloaded IntelliJ IDEA, follow these steps to install it:

Navigate to the directory where you downloaded IntelliJ IDEA.

Use the following commands to untar the downloaded IDEA archive:

## STEP2: 

```
tar -xzf ideaIC-2023.2.2.tar.gz
```

![Alt text](2.png)

After untarring, you can run IntelliJ IDEA by navigating to the installation directory and executing the idea.sh script.

## Step3: 

```
cd idea-IC-232.9921.47/bin/
```

![Alt text](3.png)

Run the the script which is named as idea.sh

## Step4:

```
./idea.sh
```

![Alt text](4.png)

Run the the script which is named as idea.sh


## Configuring IntelliJ IDEA

Once IntelliJ IDEA is running, follow these steps to configure it for Cucumber and Maven:

1. Press `Ctrl + Alt + S` to open the settings dialog box.

2. In the settings dialog box, select the **"Plugins"** option from the left menu.

3. Search for the **"Cucumber for Java"** plugin and install it.

4. Click **"Apply"** and then **"Done"** to install the plugin.

5. Next, you need to add the Maven plugin.

6. Press `Ctrl + Alt + S` again to open the settings dialog box.

7. In the settings dialog box, select the **"Plugins"** option from the left menu.

8. Search for the **"Maven"** plugin and install it.

9. Click **"Apply"** and then **"Done."**

10. Restart IntelliJ IDEA for the changes to take effect.

 ![Alt text](5.png)

![Alt text](6.png)

# Java 11 installation

To install the Java 11 version use the below command

## Step 5:

```
sudo apt install openjdk-11-jre-headless
```

![Alt text](7.png)

Check Java Version with the command java -version

![Alt text](8.png)

## Installation of Maven

To install Maven use command

## Step6:

```
sudo apt install maven
```

![Alt text](9.png)

Check Maven Version with the command mvn -version

![Alt text](10.png)

## Creating a Maven Project with Cucumber Dependencies

Follow these steps to create a Maven project with Cucumber dependencies:

1. Open your terminal.

2. Navigate to the location where you want to create the project.

3. Use the following command to create a Maven project with Cucumber dependencies:

   ```bash
   mvn archetype:generate \
     "-DarchetypeGroupId=io.cucumber" \
     "-DarchetypeArtifactId=cucumber-archetype" \
     "-DarchetypeVersion=7.12.1" \
     "-DgroupId=search" \
     "-DartifactId=search" \
     "-Dpackage=Search" \
     "-Dversion=1.0.0-SNAPSHOT" \
     "-DinteractiveMode=false"
     ```
![Alt text](11.png)

## Changes in POM.xml

### Dependency Section
The `<dependencies>` section in the `pom.xml` file is used to declare the dependencies required for your project. Dependencies are external libraries or modules that your project relies on. Maven resolves and manages these dependencies automatically. The main function of this section is that when a project moves from one environment to another then there may be a mismatch of versions of installed software and there will be a chance of error due to mismatch so dependencies section will download them and the project will run smoothly without error due to mismatch.

### Step 8: Add the Following Dependencies

Add the following code snippet under the `<dependencies>` section in your `pom.xml` file:

```xml
<dependency>
  <groupId>io.cucumber</groupId>
  <artifactId>cucumber-java</artifactId>
  <scope>test</scope>
</dependency>
```
![Alt text](12.png)

## Plugin Section

Some changes can be required in Plugins, which are done through the POM file. Under the source configuration, we need to add the Java version which is running on your machine.

### Step 9: Configure Maven Compiler Plugin

The `<plugins>` section within the `<build>` section is used to configure Maven plugins. Plugins extend Maven's functionality and enable tasks like compiling code, running tests, packaging, and more. Here's an example:

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.11.0</version>
      <configuration>
        <encoding>UTF-8</encoding>
        <source>11</source>
        <target>11</target>
      </configuration>
    </plugin>
    <!-- Add other plugins as needed -->
  </plugins>
</build>
```
![Alt text](13.png)

## Search.feature

STEP10:Now create a file under src/test/resources/Search Search.feature.  And start writing your feature file:

## Adding Step Definitions Automatically (Auto Generation of Packages, Classes, and Methods)

**Navigate to a Step in the Feature File** 

Open your feature file in IntelliJ IDEA. Place the cursor on one of the highlighted steps (Given, When, or Then) that lack step definitions.

**Generate Step Definitions**

When you place the cursor on an undefined step, a yellow bulb icon should appear to the left of the step. Click on this bulb icon.


```bash
 Feature: Upload Picture
#Maximum size 2mb
  Scenario: user is uploading picture less then 2mb
    Given user is on the registration page
    And   have upload option
    When the user click on choose option
    And select picture
    And click on upload button
    Then system should be able to upload button
```

![Alt text](14.png)

# Adding Step Definitions Automatically (Auto Generation of Packages, Classes, and Methods)

## Navigate to a Step in the Feature File

Open your feature file in IntelliJ IDEA. Place the cursor on one of the highlighted steps (Given, When, or Then) that lack step definitions.

## Generate Step Definitions

When you place the cursor on an undefined step, a yellow bulb icon should appear to the left of the step. Click on this bulb icon.

![Alt text](15.png)

## Choose "Create All Step Definitions"

A menu will appear when you click on the bulb icon. Choose "Create All Step Definitions."

## Select Step Definitions Location

IntelliJ IDEA will prompt you to choose the location where you want to create the step definitions. You can choose an existing package or create a new one. Select the appropriate option.

## Generate Step Definitions

After selecting the location, IntelliJ IDEA will generate the step definition classes and methods for all the undefined steps in your feature file.

### Step Definitions.java

It will generate the following StepDefinitions.java file under `src/test/java/Search` as I have entered to be created here.

![Alt text](16.png)

## Testing

Now test this using the Maven run feature. Here are the steps:

1. Right-click on the `RunCucumberTest.java` file.

2. Select "Run Maven," then select "Test RunCucumberTest" and click on it.

 ![Alt text](17.png)  

![Alt text](18.png)

By following these steps, you can automatically generate step definitions for your Cucumber feature file steps in IntelliJ IDEA. This can significantly speed up the process of creating step definitions for your scenarios.
