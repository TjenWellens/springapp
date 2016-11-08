# Configuration
Make sure that:
- pom.xml:`project.build.plugins.plugin.configuration.server` == ~/.m2/settings.xml:`settings.servers.server.id`
- tomcat-docker/tomcat-users.xml:`tomcat-users.user#username` == ~/.m2/settings.xml:`settings.servers.server.username`
- tomcat-docker/tomcat-users.xml:`tomcat-users.user#username` == ~/.m2/settings.xml:`settings.servers.server.password`
- tomcat-docker/tomcat-users.xml:`tomcat-users.user#roles` should contain `manager-script` (for tomcat7)
- pom.xml:`project.build.plugins.plugin.configuration.url` should be like `<host>:<port>/manager/text` (for tomcat7)

# Sample snippets
~/.m2/settings.xml
```
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
											https://maven.apache.org/xsd/settings-1.0.0.xsd">
	<localRepository/>
	<interactiveMode/>
	<usePluginRegistry/>
	<offline/>
	<pluginGroups/>
	<servers>
		<server>
			<id>tomcat-docker</id>
			<username>mgrscript</username>
			<password>mgrscript</password>
		</server>
	</servers>
	<mirrors/>
	<proxies/>
	<profiles/>
	<activeProfiles/>
</settings>
```

tomcat-docker/tomcat-users.xml
```
<tomcat-users>
  <!--manager-gui is needed to access <url>/manager/html-->
  <role rolename="manager-gui"/>
  <!--manager-script is needed to access <url>/manager/text-->
  <role rolename="manager-script"/>

  <user username="admin" password="admin" roles="manager-gui"/>
  <user username="mgrscript" password="mgrscript" roles="manager-script"/>
</tomcat-users>
```

pom.xml
```
<project>
  ...
  <build>
    <finalName>${project.artifactId}</finalName>
    <plugins>
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.2</version>
        <configuration>
          <url>http://localhost:8080/manager/text</url>
          <server>tomcat-docker</server>
          <path>/${project.artifactId}</path>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```
