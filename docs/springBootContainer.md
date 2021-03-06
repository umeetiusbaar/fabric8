## Spring Boot container

Fabric comes with a set of features simplifying the effort of running and managing Spring Boot JVM processes. Fabric
Boot utilities and starters are especially useful if you plan to run your system in a microservices-manner backed by
the Spring Boot micro-containers and Fabric-related middleware (Camel, ActiveMQ, CXF and so forth).

### Spring Boot Quickstarts

Before we dig into the glorious details of the Fabric8 support for the Spring Boot applications, keep in mind that the 
easiest way to start developing Spring Boot applications with Fabric8 is to use *Quickstarts*. Quickstarts are our 
opinionated Maven Archtypes that we highly recommend to use as a base for your projects.

For example to create and run the basic Spring Boot REST module follow these steps:

**Step 1:** Create the new Maven project from the Quickstart.

    $ mvn archetype:generate -Dfilter=io.fabric8.archetypes:springboot-webmvc-archetype 

After executing the command above, you should see a similar output:

    [INFO] Scanning for projects...
    ...
    [INFO] >>> maven-archetype-plugin:2.2:generate (default-cli) @ standalone-pom >>>
    [INFO] Generating project in Interactive mode
    Choose archetype:
    1: remote -> io.fabric8.archetypes:springboot-webmvc-archetype (Creates a new Shows how to use Spring Boot with WebMVC in the Java Container)
    Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains): : 1
    Define value for property 'groupId': : com.myapp    
    Define value for property 'artifactId': : my-spring-boot-rest-module
    Define value for property 'version':  1.0-SNAPSHOT: : 
    Define value for property 'package':  com.myapp: : 
    ...
    [INFO] project created from Archetype in dir: /home/hekonsek/tmp/my-spring-boot-rest-module
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------

**Step 2:** Test and install your new module.

In the previous step we created new maven project with artifactId `my-spring-boot-rest-module`. Let's step into a 
directory of our new module and build it, using the following commands:

    $ cd my-spring-boot-rest-module 
    $ mvn install
    
After executing the commands above, you should see a similar output:
    
    [INFO] Scanning for projects...
    ... 
    [INFO] Building Fabric8 :: Quickstarts :: Spring-Boot :: WebMVC 1.0-SNAPSHOT
    ...
    [INFO] --- maven-compiler-plugin:2.3.1:compile (default-compile) @ my-spring-boot-rest-module ---
    [INFO] Compiling 4 source files to /home/hekonsek/tmp/my-spring-boot-rest-module/target/classes
    ...
    T E S T S
    -------------------------------------------------------
    Running com.myapp.InvoicingRestApiTest
    ...
      .   ____          _            __ _ _
     /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
    ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
     \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
      '  |____| .__|_| |_|_| |_\__, | / / / /
    =========|_|==============|___/=/_/_/_/
    :: Spring Boot ::        (v1.1.2.RELEASE)
    ...
    2014-06-30 23:43:36.178  INFO 9719 --- [ost-startStop-1] org.hibernate.tool.hbm2ddl.SchemaExport  : HHH000230: Schema export complete
    ...
    2014-06-30 23:43:37.434  INFO 9719 --- [           main] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat started on port(s): 36063/http
    2014-06-30 23:43:37.437  INFO 9719 --- [           main] o.a.maven.surefire.booter.ForkedBooter   : Started ForkedBooter in 7.385 seconds (JVM running for 8.605)
    2014-06-30 23:43:37.556  INFO 9719 --- [o-auto-1-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring FrameworkServlet 'dispatcherServlet'
    ...
    Tests run: 2, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 8.797 sec
    ...
    Results :
    Tests run: 2, Failures: 0, Errors: 0, Skipped: 0
    ...
    [INFO] --- maven-install-plugin:2.4:install (default-install) @ my-spring-boot-rest-module ---
    [INFO] Installing /home/hekonsek/tmp/my-spring-boot-rest-module/target/my-spring-boot-rest-module-1.0-SNAPSHOT.jar to /home/hekonsek/.m2/repository/com/myapp/my-spring-boot-rest-module/1.0-SNAPSHOT/my-spring-boot-rest-module-1.0-SNAPSHOT.jar
    [INFO] Installing /home/hekonsek/tmp/my-spring-boot-rest-module/pom.xml to /home/hekonsek/.m2/repository/com/myapp/my-spring-boot-rest-module/1.0-SNAPSHOT/my-spring-boot-rest-module-1.0-SNAPSHOT.pom
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------


**Step 3:** Start Fabric8 server.

Now we need to start the Fabric8 server which we would like to provision and manage our brand new Spring Boot
application.

    $ cd fabric8-karaf-1.1.0
    $ bin/fabric8
    
After executing the commands above, you should see a similar output:
    
    Please wait while Fabric8 is loading...
    99% [=======================================================================>]
    ______    _          _      _____ 
    |  ___|  | |        (_)    |  _  |
    | |_ __ _| |__  _ __ _  ___ \ V / 
    |  _/ _` | '_ \| '__| |/ __|/ _ \ 
    | || (_| | |_) | |  | | (__| |_| |
    \_| \__,_|_.__/|_|  |_|\___\_____/
    Fabric8 Container (1.1.0)
    http://fabric8.io/
    
    Fabric8:karaf@root> 

**Step 4:** Deploy profile of your application into the Fabric8.

With the Fabric8 server up and running we can go back to the directory of our new project and execute the following Maven
goal in order to deploy the profile describing our Spring application:
 
    .../my-spring-boot-rest-module $ mvn fabric8:deploy -DskipTests
    
After executing the command above, you should see a similar output:
    
    [INFO] Uploading file /home/hekonsek/tmp/my-spring-boot-rest-module/pom.xml
    Downloading: http://192.168.2.12:8181/maven/upload/com/myapp/my-spring-boot-rest-module/1.0-SNAPSHOT/maven-metadata.xml
    ...
    Uploading: http://192.168.2.12:8181/maven/upload/com/myapp/my-spring-boot-rest-module/1.0-SNAPSHOT/my-spring-boot-rest-module-1.0-20140630.215711-1.jar
    Uploaded: http://192.168.2.12:8181/maven/upload/com/myapp/my-spring-boot-rest-module/1.0-SNAPSHOT/my-spring-boot-rest-module-1.0-20140630.215711-1.jar (5 KB at 63.0 KB/sec)
    ...
    [INFO] Updating profile: quickstarts-spring.boot-webmvc with parent profile(s): [containers-java.spring.boot]
    ...
    [INFO] Profile page: http://192.168.2.12:8181/hawtio/index.html#/wiki/branch/1.0/view/fabric/profiles/quickstarts/spring.boot/webmvc.profile
    ...
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    
Now your Fabric8 configuration registry contains the profile describing your Spring Boot application. In the next step 
we will use that profile to provision our Spring application.

**Step 5:** Provision your Spring Boot container as a standalone JVM process managed by the Fabric8.

    Fabric8:karaf@root> container-create-child --profile quickstarts-spring.boot-webmvc root my-spring-boot-rest
    
After executing the command above, you should see a similar output:
    
    Starting process
    Running java  -javaagent:jolokia-agent.jar=host=0.0.0.0,port=9018,agentId=my-spring-boot-rest ... 
    process is now running (11904)
    The following containers have been created successfully:
	    Container: my-spring-boot-rest.
    
**Step 6:** Find the HTTP port number assigned by Fabric8 to your application.

    Fabric8:karaf@root> environment my-spring-boot-rest | grep HTTP_PROXY
    FABRIC8_HTTP_PROXY_PORT                       9017

**Step 7:** Enjoy your application being up and running!

    $ curl http://localhost:9017/  
    {
        "_links" : {
            "invoice" : {
                "href" : "http://localhost:9017/invoice{?page,size,sort}",
                "templated" : true
            }
        }
    }%      

### Fabric8 Spring Boot BOM

The best way to manage Spring Boot dependencies in the Fabric8-managed application is to import the Fabric8 Spring Boot
BOM.

    <dependencyManagement>
      <dependencies>
        <dependency>
          <groupId>io.fabric8</groupId>
          <artifactId>process-spring-boot-bom</artifactId>
          <version>${fabric8.version}</version>
          <type>pom</type>
          <scope>import</scope>
        </dependency>
      </dependencies>
    </dependencyManagement>

If your POM is armed with the BOM definition presented above, you can import Spring Boot and related Fabric8 jars into
your microservice without specifying the versions of these dependencies.

    <dependencies>
      <dependency>
        <groupId>io.fabric8</groupId>
        <artifactId>process-spring-boot-container</artifactId>
      </dependency>
      <dependency>
        <groupId>io.fabric8</groupId>
        <artifactId>process-spring-boot-starter-camel</artifactId>
      </dependency>
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
      </dependency>
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
      </dependency>
      <dependency>
        <groupId>org.springframework.data</groupId>
        <artifactId>spring-data-rest-webmvc</artifactId>
      </dependency>
    </dependencies>

### FabricSpringApplication

`FabricSpringApplication` is an executable Java class to be used as a base for the Fabric-managed Spring Boot applications. Its main purpose is to
eliminate the custom code bootstrapping the application, so end-users could create Spring Boot managed process via
Fabric without any custom wiring.

`FabricSpringApplication` can be used in the conjunction with the Fabric Jar Managed Process installer (just
 as demonstrated on the snippet below).

     process:install-jar -m io.fabric8.process.spring.boot.container.FabricSpringApplication my.group.id my-artifact 1.0

 Keep in mind that you don't have to use `FabricSpringApplication` in order to use Fabric goodies for Spring
 Boot (like Fabric starters). However we recommend to use this class as an entry point for your Fabric SpringBoot
 integration, as it implements our opinionated view of the proper Fabric+Boot wiring.

 In order to specify packages that should be scanned for additional `@Component` and `@Configuration` classes, use
 standard Spring Boot `spring.main.sources` system property. For example if your project `@Configuration` classes are located in
 the `com.example.project` package, you can use the following command to install your jar as a managed process:

      process:install-jar -m io.fabric8.process.spring.boot.container.FabricSpringApplication --jvm-options=-Dspring.main.sources=com.example.project my.group.id my-artifact 1.0

### Embedded FabricSpringApplication

Sometimes you cannot start new Spring Boot JVM process, but instead you have to integrate with the existing web application
or the other piece of legacy Spring software. To support such cases Fabric comes withe the
`EmbeddedFabricSpringApplication`, a bean that can be added to the existing Spring application context in order to start
embedded Fabric Spring Boot context from within it. The embedding context (the one `EmbeddedFabricSpringApplication` has
been added to will become a parent context for the embedded Fabric Spring Boot context.

Creating embedded Fabric application is as simple as that:

    @Bean
    EmbeddedFabricSpringApplication fabricSpringApplication() {
      return new EmbeddedFabricSpringApplication();
    }

For XML configuration the snippet above looks as follows:

    <bean class="io.fabric8.process.spring.boot.container.EmbeddedFabricSpringApplication" />

`EmbeddedFabricSpringApplication` automatically detects its patent Spring `ApplicationContext` and uses it when starting
up new embedded Fabric application.

### Provisioning Spring Boot applications as Fabric8 Java Containers

The recommended way to provision Spring Boot applications in the Fabric8 environment is to install them as
[Java Containers](javaContainer.html). This way your Spring Boot
application will be treated by the Fabric8 as the any other container, so you could take advantage of the Hawtio, ZooKeeper
runtime registry, [Gateway](gateway.html) and many other useful Fabric8 features.

In the following paragraphs we will demonstrate how to use the Fabric8 Java Container to provision the
[Spring Boot invoicing microservice demo](https://github.com/fabric8io/fabric8/tree/master/process/process-spring-boot/process-spring-boot-itests/process-spring-boot-itests-service-invoicing).

First of all - create new profile for your application. We will name our profile `invoicing`, because it describes
microservice related to the *invoicing* business concern.

    > profile-create --parents containers-java.spring.boot invoicing

Then add your microservice jar to that profile:

    > profile-edit --pid=io.fabric8.container.java/jarUrl=mvn:io.fabric8/process-spring-boot-itests-service-invoicing/1.1.0-SNAPSHOT invoicing
    Setting value:mvn:io.fabric8/process-spring-boot-itests-service-invoicing/1.1.0-SNAPSHOT key:jarUrl on pid:io.fabric8.container.java and profile:invoicing version:1.0

In order to find `@Component` and `@Configuration` classes specific to your application, set the `spring.main.sources`
system property:

    > profile-edit --pid=io.fabric8.container.java/jvmArguments=-Dspring.main.sources=io.fabric8.process.spring.boot.itests.service.invoicing invoicing
    Setting value:-Dspring.main.sources=io.fabric8.process.spring.boot.itests.service.invoicing key:jvmArguments on pid:io.fabric8.container.java and profile:invoicing version:1.0

Our `invoice` profile is all set now - we can finally provision our microservice as a Java Container:

    > container-create-child --profile invoicing root invoicing
    Starting process
    Running java -Dspring.main.sources=...
    ...
    process is now running (22074)
    The following containers have been created successfully:
    	Container: invoicing.

The container seems to be started properly, but let's verify this by listing REST services available under
`http://localhost:8080` address:

    ~/fabric8-karaf-1.1.0-SNAPSHOT [10001]% curl http://localhost:8080/
    {
      "_links" : {
        "invoice" : {
          "href" : "http://localhost:8080/invoice{?page,size,sort}",
          "templated" : true
        }
      }
    }%

Our invoicing microservice has been successfully started and exposed under the following URL -
`http://localhost:8080/invoice` . As we can see our invoicing service has been deployed as regular Fabric8 child
container.

     > container-list
     [id]                           [version] [connected] [profiles]                                         [provision status]
     root*                          1.0       true        fabric, fabric-ensemble-0000-1                     success
       invoicing                    1.0       true        invoicing                                          success

And at the same time it is visible for the Fabric8 as a managed process:

    > process:ps
    [id]                     [pid] [name]
    invoicing                8576  java io.fabric8.process.spring.boot.container.FabricSpringApplication

### Profile containers-java.spring.boot

You may be wondering what is the `containers-java.spring.boot` profile we used in example above.

    > profile-create --parents containers-java.spring.boot invoicing

This is opinionated base profile for Spring Boot applications provisioned using Fabric8. Why should you use it? Our
Spring Boot profile comes with some useful default settings that we recommend to have in place when deploying Spring
Boot applications.

First of all our `containers-java.spring.boot` profile specifies the main class of your application. It uses
`io.fabric8.process.spring.boot.container.FabricSpringApplication` main class - the opinionated Spring Boot application
configuration for processes provisioned and managed by the Fabric8.

### Spring Boot Camel starter

Fabric Spring Boot support comes with the opinionated auto-configuration of the Camel context. It provides default
`CamelContext` instance, auto-detects Camel routes available in the Spring context and exposes the key Camel utilities
(like consumer template, producer template or type converter service).

In order to start using Camel with Spring Boot just include `process-spring-boot-starter-camel` jar in your application
classpath.

    <dependency>
      <groupId>io.fabric8</groupId>
      <artifactId>process-spring-boot-starter-camel</artifactId>
      <version>${fabric-version}</version>
    </dependency>

When `process-spring-boot-starter-camel` jar is included in the classpath, Spring Boot will auto-configure the Camel
context for you.

#### Auto-configured CamelContext

The most important piece of functionality provided by the Camel starter is `CamelContext` instance. Fabric Camel starter
will create `SpringCamelContext` for your and take care of the proper initialization and shutdown of that context. Created
Camel context is also registered in the Spring application context (under `camelContext` name), so you can access it just
as the any other Spring bean.

    @Configuration
    public class MyAppConfig {

        @Autowired
        CamelContext camelContext;

        @Bean
        MyService myService() {
          return new DefaultMyService(camelContext);
        }

    }

Camel starter collects all the `RoutesBuilder` instances from the Spring context and automatically injects
them into the provided `CamelContext`. It means that creating new Camel route with the Spring Boot starter is as simple as
adding the `@Component` annotated class into your classpath:

    @Component
    public class MyRouter extends RouteBuilder {

      @Override
      public void configure() throws Exception {
        from("jms:invoices").to("file:/invoices");
      }

    }

Or creating new route `RoutesBuilder` in your `@Configuration` class:

    @Configuration
    public class MyRouterConfiguration {

      @Bean
      RoutesBuilder myRouter() {
        return new RouteBuilder() {

          @Override
          public void configure() throws Exception {
            from("jms:invoices").to("file:/invoices");
          }

        };
      }

    }

#### Auto-configured ActiveMQ client

Fabric comes with the auto-configuration class for the ActiveMQ client. It provides a pooled ActiveMQ
`javax.jms.ConnectionFactory`. In order to use the `ActiveMQAutoConfiguration` in your application just add the
following jar to your Spring Boot project dependencies:

    <dependency>
      <groupId>io.fabric8</groupId>
      <artifactId>process-spring-boot-starter-activemq</artifactId>
      <version>${fabric-version}</version>
    </dependency>

Fabric's ActiveMQ starter will provide the default connection factory for you.

    @Component
    public class InvoiceReader {

      private final ConnectionFactory connectionFactory;

      @Autowired
      public InvoiceReader(ConnectionFactory connectionFactory) {
        this.connectionFactory = connectionFactory;
      }

      void ProcessNextInvoice() {
        Connection invoiceQueueConnection = connectionFactory.createConnection();
        // invoice processing logic here
      }

    }

Keep in mind that if you include the
[spring-jms](http://docs.spring.io/spring/docs/4.0.x/spring-framework-reference/html/remoting.html#remoting-jms) jar
together with the ActiveMQ starter in your Spring Boot application classpath, `JmsTemplate` will be automatically
populated with the ActiveMQ connection factory.

    <dependency>
      <groupId>io.fabric8</groupId>
      <artifactId>process-spring-boot-starter-activemq</artifactId>
      <version>${fabric-version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
     <artifactId>spring-jms</artifactId>
      <version>${spring-version}</version>
    </dependency>

Now you can use `JmsTemplate` with ActiveMQ without explicit configuration.

    @Component
    public class InvoiceReader {

      private final JmsTemplate jmsTemplate;

      @Autowired
      public InvoiceReader(JmsTemplate jmsTemplate) {
        this.jmsTemplate = jmsTemplate;
      }

      void ProcessNextInvoice() {
        String invoiceId = (String) jmsTemplate.receiveAndConvert("invoices");
        // invoice processing logic
       }

    }

