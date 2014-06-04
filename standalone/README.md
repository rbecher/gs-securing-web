# Using Spring Security with a standalone Tomcat

## The problem

I want Tomcat (or rather the admin) to handle the user management.
In Spring I just want to secure some endpoints (index.html and two RESTful controllers) and bind it to either a user or, even better, a group/role.

I want to avoid using xml config inside Spring and use just classes and annotations.
Xml config in tomcat is ok :-)

Booting up spring I see a log entry
> 2014-06-04 11:37:31.676  INFO 27296 --- [on(2)-127.0.0.1] b.a.s.AuthenticationManagerConfiguration : 
>  
>  Using default password for application endpoints: 543fbdb8-c5d0-4b1b-b7ce-cd6fd008520a
>
> 2014-06-04 11:37:31.775  INFO 27296 --- [on(2)-127.0.0.1] o.s.s.web.DefaultSecurityFilterChain     : Creating filter chain: org.springframework.security.web.util.matcher.AnyRequestMatcher@1, [org.springframework.security.web.context.request.async.WebAsyncManagerIntegrationFilter@5867ecdd, ...
>
> ...
>
> 2014-06-04 11:37:31.794  INFO 27296 --- [on(2)-127.0.0.1] o.s.b.c.embedded.FilterRegistrationBean  : Mapping filter: 'springSecurityFilterChain' to: [/*]
>
> ... 
>
> 2014-06-04 11:37:31.907  INFO 27296 --- [on(2)-127.0.0.1] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/login] onto handler of type [class org.springframework.web.servlet.mvc.ParameterizableViewController]

Trying to call upon the secured route I get a login form, yet using the information from the *tomcat-users.xml* I get an error **Invalid username and password.**.
 
So what am I missing? 

## Setup

I have 

* vanilla Apache Tomcat 8.0.8
* a modified *conf/tomcat-users.xml*
* a modified *conf/server.xml*
* a cloned deployable version of the [Securing Spring Web Guide](https://spring.io/guides/gs/securing-web) in my local branch and in this folder (intellij module), see [github repo](https://github.com/rbecher/gs-securing-web/tree/standalone/standalone)

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