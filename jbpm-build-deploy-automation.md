1. `git clone` the latest version of the jBPM project to the local file system in the build server
2. Add the following lines to project's POM.xml to specify the details of the Maven repository into which the project's JAR needs to be published. 

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

3. Add the following configuration in settings.xml (_located at ~/.m2 of the host files system in the build server_) to specify the connection details to the repository server (Business Central) that hosts the Maven repository. 

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
 4. Finally run `mvn deploy` from the location at which the project has been checked out and the JAR file will get uploaded into the Maven repository.
 
 5. Deploy the JAR installed into BC in step 4, using the below listed controller REST API.
 
 ```
 http://localhost:8080/business-central/rest/controller/management/servers/kieserver/containers/citi-test_1.0.0

##Payload

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<container-spec-details>
    <container-id>citi-test_1.0.0</container-id>
    <container-name>citi-test</container-name>
    <server-template-key>
        <server-id>kieserver</server-id>
        <server-name>kieserver</server-name>
    </server-template-key>
    <release-id>
        <artifact-id>citi-test</artifact-id>
        <group-id>com.bala</group-id>
        <version>1.0.0</version>
    </release-id>
    <configs>
        <entry>
            <key>RULE</key>
            <value xsi:type="ruleConfig" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
                <scannerStatus>STOPPED</scannerStatus>
            </value>
        </entry>
        <entry>
            <key>PROCESS</key>
            <value xsi:type="processConfig" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
                <runtimeStrategy>PER_PROCESS_INSTANCE</runtimeStrategy>
                <kbase></kbase>
                <ksession></ksession>
                <mergeMode>MERGE_COLLECTIONS</mergeMode>
            </value>
        </entry>
    </configs>
    <status>STARTED</status>
</container-spec-details>
```
