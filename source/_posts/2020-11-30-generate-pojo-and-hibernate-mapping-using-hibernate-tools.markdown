---
layout: post
title: "Generate POJO and Hibernate Mapping Using Hibernate Tools"
date: 2020-11-30 18:13:54 +0700
comments: true
categories: [java, eclipse]
---

Sometime we get into a condition in which we need to create POJO classes from an existing database. It could  take time if the database has many tables. Instead of manually create them, we could use [Hibernate Tool](https://docs.jboss.org/tools/4.1.0.Final/en/hibernatetools/html_single/index.html). Hibernate Tools is an eclipse plugin that can be installed from the eclipse marketplace. 

In this post, we use Spring Tool Suite (STS) 4 instead of directly use the eclipse version of the IDE. Meanwhile the database we use is PostgreSQL. It should be applicable to any other database.

The steps of using Hibernate Tools is summarized as follow. 
I append many screenshots on this post in order the steps are easier to follow.

#### Step 1 : Install Hibernate Tools plugin

To install Hibernate Tools plugin, go to menu <code> Help > Eclipse Marketplace </code>.

{% img center /images/post/2020-11-30-fig01.png %}

Enter <code>Hibernate Tools</code> in the search field.

{% img center /images/post/2020-11-30-fig02.png %}

Follow the installation step until the installation success and finish. Eclipse will show us a pop up message to restart it once the installation finished.

#### Step 2 : Create Java Maven project
Before building a connection to the database, we will create a maven project that contains a hibernate configuration file and where the generated code will be placed. The following figures show the maven project configuration setting. 

First, ensure we are in the <code>Java persepective</code> mode. From the menu <code>File > New > Other...</code> select <code>Maven Project</code> as the following figure. 

{% img center /images/post/2020-11-30-fig03.png %}

Select the checked box of <code>Create a simple project</code> then press <code>Next</code>.

{% img center /images/post/2020-11-30-fig04.png %}

Fill the <code>Group Id</code> and <code>Artifact Id</code>. Select <code>jar</code> for the <code>Packaging</code> option, then click <code>Finish</code>

{% img center /images/post/2020-11-30-fig05.png %}

Create a namespace in the maven project where the generated code will be placed.
{% img center /images/post/2020-11-30-fig06.png %}

The project structure should look like the following picture.
{% img center /images/post/2020-11-30-fig07.png %}

Once the project skeleton constructued, edit the <code>pom.xml</code> by adding dependencies of the hibernate and database driver. The latest jar version could be found in [Maven Repository](https://mvnrepository.com/)


``` xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>io.neutrofoton.lab</groupId>
  <artifactId>hibernatereverse</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  
 <dependencies>
 
 	<dependency>
	    <groupId>org.hibernate</groupId>
	    <artifactId>hibernate-core</artifactId>
	    <version>5.4.24.Final</version>
	</dependency>
	
	<dependency>
	    <groupId>org.postgresql</groupId>
	    <artifactId>postgresql</artifactId>
	    <version>42.2.18</version>
	</dependency>
		
 	
 </dependencies>
</project>

```

#### Step 3 : Create Hibernate configuration
Before creating Hibernate configuration file, we need to switch to <code>Hibernate perspective</code> in the eclipse by opening menu <code>Window > Perspective > Open Perspective > Other...</code>. Then select <code>Hibernate</code> on the perspective list.

{% img center /images/post/2020-11-30-fig08.png %}

Once we are in the <code>Hibernate perspective</code>, we can create Hibernate configuration from menu <code>File > Hibernate Configuration File (*.cfg.xml)</code>

{% img center /images/post/2020-11-30-fig09.png %}

Placed the configuration file in the directory <code>src/main/java</code>, then click <code>Next</code>.

{% img center /images/post/2020-11-30-fig10.png %}

On the next dialog wizard, fill the configuration item according to our database specification. The database specification items to fill are database dialect, database driver, connection URL, default schema (if we want to generate for a specific schema, otherwise left it empty if we want to generate for all), username and password. On the dialog wizard, ensure to select the check box of <code>Create a console configuration</code> option.

{% img center /images/post/2020-11-30-fig11.png %}

In the <code>Hibernate Configurations</code> pane, the hibernate configuration that has been created should be listed in it. If the created hibernate configuration is not listed, press the <code>Refresh or Rebuild configuration</code> button in the top right corner of the pane. The generated Hibernate configuration should look like the following figure. We could expand the Database element on the Hibernate configuration to ensure we have a valid database connection.

{% img center /images/post/2020-11-30-fig12.png %}

#### Step 4 : Run Hibernate code generation
To run Hibernate code generation, ensure we select the active <code>Hibernate Configuration</code> in the Hibernate configuration pane. Then, open the menu of <code>Run > Hibernate Code Generation Configuration...</code>

{% img center /images/post/2020-11-30-fig13.png %}

In the <code>Hibernate Code Generation Configuration</code>, fill the package where the domain class will be in.
{% img center /images/post/2020-11-30-fig14.png %}

In the <code>Exporter</code> tab, select <code>Use Java 5 syntax</code> and <code>Generate EJB3 annotations</code>. In the exporter list option, we can select items in the list that we need. In this post we only select <code>Domain code</code> that has annotation as Hibernate mapping. Finally click <code>Apply</code> and <code>Run</code> to generate the domain code.

{% img center /images/post/2020-11-30-fig15.png %}

### NOTES:
If the annotation does not generated in the domain class, open the <code>Hibernate Configuration</code> and edit it. Then change the <code>Hibernate Version</code> to <code>5.2</code>. Finally run again as we have done on **step 4**.

{% img center /images/post/2020-11-30-fig16.png %}

{% img center /images/post/2020-11-30-fig17.png %}


# References
1. https://docs.jboss.org/tools/4.1.0.Final/en/hibernatetools/html_single/index.html
2. https://stackoverflow.com/questions/50837574/annotation-not-created-when-generating-hibernate-mapping-files
3. https://www.youtube.com/watch?v=KO_IdJbSJkI&ab_channel=CodeJava
