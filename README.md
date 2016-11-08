Following the [spring framework tutorial](https://docs.spring.io/docs/Spring-MVC-step-by-step), but suddenly decided to change things up:
- ant -> maven
- docker tomcat server


## Standard workflow
in separate window:
```
docker run -it --rm -p=8080:8080 -v $(pwd)/target/springapp:/usr/local/tomcat/webapps/springapp tjenwellens/tomcat
```

in main window
`mvn package`

browser `http://localhost:8080/manager/html` click 'reload'

## Debugging
in separate window:
```
docker run -it --rm -p=8080:8080 -p=1043:1043 -e JPDA_ADDRESS=1043 -v $(pwd)/target/springapp:/usr/local/tomcat/webapps/springapp tjenwellens/tomcat /bin/sh -c "catalina.sh jpda start; tail -F -n0 /etc/hosts"
```

in main window:
`mvn package`

browser `http://localhost:8080/manager/html` click 'reload'

## Without mounting folder
in separate window:
```
docker run -it --rm --publish=8080:8080 tjenwellens/tomcat
```

in main window:
`mvn clean package tomcat7:deploy`
