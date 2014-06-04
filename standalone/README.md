# Using Spring Security with a standalone Tomcat

## The problem

I want Tomcat (or rather the admin) to handle the user management.
In Spring I just want to secure some endpoints (index.html and two RESTful controllers) and bind it to either a user or, even better, a group/role.

I want to avoid using xml config inside Spring and use just classes and annotations.
Xml config in tomcat is ok :-)

## Setup

I have 

* vanilla Apache Tomcat 8.0.8
* a modified *conf/tomcat-users.xml*
* a modified *conf/server.xml*
* a deployable version of the [Securing Spring Web Guide](https://spring.io/guides/gs/securing-web) in my local branch and in this folder (intellij module)

## Configuration

### Spring
See the *WebSecurityConfig* class.

### tomcat-users.xml

    <!-- ... --->
    <role rolename="manager-gui"/>
    <role rolename="manager-script"/>
    <role rolename="manager-jmx"/>
    <role rolename="manager-status"/>
    <role rolename="ROLE_stakeholder"/>
    <user username="stakeholder" password="Foobarbaz" roles="ROLE_stakeholder,manager-gui,manager-script,manager-jmx,manager-status"/>
    <!-- ... --->
    
### server.xml

    <!-- ... --->
    <Engine name="Catalina" defaultHost="localhost">
        <!-- ... --->
        <Realm className="org.apache.catalina.realm.MemoryRealm" />
    </Engine>