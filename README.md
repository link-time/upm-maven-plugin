# UPM Maven Plugin
Provides Maven goals to interact with Atlassian UPM REST API.

## Configuration
```
<plugin>
    <groupId>org.link-time.maven.plugin.atlassian</groupId>
    <artifactId>upm-maven-plugin</artifactId>
    <version>VERSION</version>
    <configuration>
        <!-- mandatory parameters -->
        <baseUrl>https://jira.example.com</baseUrl>
        <username>admin</username>
        <password>admin</password>
        <!-- optional parameters -->
        <timeoutMillis>10000</timeoutMillis>
    </configuration>
</plugin>
```

## Upload plugin JAR file
```
mvn upm:uploadPluginFile -DpluginFile="my-plugin-1.0.0.jar"
```
Plugin file path is either absolute or relative to `pom.xml` directory.

### Optional parameters

| Parameter | Default value | Purpose |
| --------- | ------------- | ------- |
| waitForInstallationMillis | 5000 | How long to wait for installation after successful file upload |

Note that we cannot detect whether the plugin was installed successfully.
We only know whether the file was accepted. We assume successful installation
after `waitForInstallationMillis` milliseconds.

## Trigger re-index
Atlassian Jira will complain if not re-indexed after changes to plugins.
It is therefore a good idea to trigger a background re-index immediately
after plugin installation.

Caveat: Reindex is pointless if triggered before plugin installation finishes.
To avoid this, using `waitForInstallationMillis` when uploading the plugin file
is our best tool for now.

```
mvn upm:reindex
```

### Optional parameters

| Parameter | Default value | Purpose |
| --------- | ------------- | ------- |
| waitForSuccessMillis | 60000 | How long to wait for reindex to finish; Build will still succeed if exceeded |