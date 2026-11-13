# Java Project Template using Maven

A Java Project Template using Maven.

This project template is assumed to be used internally on my local CI/CD environment. It can be done by
- Using internal Nexus as maven repository
  - store build files (JAR, WAR)
  - store docker images 
- Using internal Jenkins to build as normal CI practices
- Using internal Forgejo as source version control 

## How to use this project template

1. Create a new repo using this template or clone it
2. Rename the JenkinsTemplate to Jenkinsfile
3. Update the image name in the Jenkinsfile (Line 57)
4. Update the artifactId in the pom.xml (Line 13)
5. (Optional) Change the parent (Line 5 - 9)
6. (Optional) Create a Dockerfile
7. Update this README.md file
8. Update the CHANGELOG.md

## How to build

### Build locally

1. Create a settings.xml file under user maven home directory. (e.g. ~/.m2)
    - Add your nexus credential
    ```
    <settings>
    <servers>
      <server>
        <id>nexus3-local-saturn</id>
        <username></username> <!-- Input your nexus account -->
        <password></password> <!-- Input your nexus password -->
      </server>
    </servers> 
    <mirrors>
      <mirror>
        <id>nexus3-local-saturn</id>
        <mirrorOf>*</mirrorOf>
        <name>Saturn Nexus Public Mirror</name>
        <url>https://saturn:30088/cicd/nexus/repository/maven-public/</url>
      </mirror>
    </mirrors>
    <profiles>
      <profile>
        <id>saturn-nexus-profile</id>
        <properties>
          <saturn.nexus.url>https://saturn:30088/cicd/nexus</saturn.nexus.url>
        </properties>
        <repositories>
          <repository>
            <id>nexus3-local-saturn</id>
            <url>https://saturn:30088/cicd/nexus/repository/maven-public/</url>
            <releases><enabled>true</enabled></releases>
            <snapshots><enabled>true</enabled></snapshots>
          </repository>
        </repositories>
        <pluginRepositories>
          <pluginRepository>
            <id>nexus3-local-saturn</id>
            <url>https://saturn:30088/cicd/nexus/repository/maven-public/</url>
            <releases><enabled>true</enabled></releases>
            <snapshots><enabled>true</enabled></snapshots>
          </pluginRepository>
        </pluginRepositories>
        </profile>
    </profiles>
    <activeProfiles>
      <activeProfile>saturn-nexus-profile</activeProfile>
    </activeProfiles>
    </settings>
    ```
2. Run the following command
    ```
    ./mvnw clean build install
    ```

### Build using Jenkins	
Besides rename the Jenkins template file and update the Jenkins file, some of additional setting must be done

- Create a Maven setting file in Jenkins using Jenkins Plugin (Config File Provider Plugin) in order to access the internal nexus

## Rule for pom.xml
- For artifactId, keep using the SNAPSHOT

## Rule for versioning 
- Tag format use v as prefix, such as v1.0.0 
- Update the CHANGELOG.md 