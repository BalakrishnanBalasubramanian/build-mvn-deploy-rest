1. `git clone` the latest version of the jBPM project to the local file system in the build server
2. Add the following lines to project's POM.xml to speciy the details of the Maven repository into which the project's JAR needs to be published. 

```
 <distributionManagement>  
      <repository>  
        <id>maven-build-repo</id>  
        <name>maven repo</name>  
        <url>http://localhost:8080/business-central/maven2/</url>  
        <layout>default</layout>  
      </repository>  
    </distributionManagement>
```   

3. Add the following configuration in settings.xml (_located at ~/.m2 of the host files system in the build server) to specify the connection details to the repository server (Business Central) that contains the repository. 

```   
  <server>  
      <id>maven-build-repo</id>  
      <username>pamAdmin</username>  
      <password>********</password>  
      <configuration>  
        <wagonProvider>httpclient</wagonProvider>  
        <httpConfiguration>  
          <all>  
            <usePreemptive>true</usePreemptive>  
          </all>  
        </httpConfiguration>  
      </configuration>  
    </server>
 ```   
 4. Finally run `mvn deploy` and the JAR file will get uploaded.
