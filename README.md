[![Build Status][ci-img]][ci] [![Released Version][maven-img]][maven]

# OpenTracing JDBC Instrumentation
OpenTracing instrumentation for JDBC.

## Installation

pom.xml
```xml
<dependency>
    <groupId>io.opentracing.contrib</groupId>
    <artifactId>opentracing-jdbc</artifactId>
    <version>VERSION</version>
</dependency>
```

## Usage

1. Add _tracing_ to jdbc url. E.g. _jdbc:tracing:h2:mem:test_  
To trace calls only if there is an active Span use property `traceWithActiveSpanOnly=true`.  
E.g. _jdbc:tracing:h2:mem:test?traceWithActiveSpanOnly=true_

2. Set driver class to `io.opentracing.contrib.jdbc.TracingDriver`

3. Instantiate tracer and register it with GlobalTracer
```java
// Instantiate tracer
Tracer tracer = ...

// Register tracer with GlobalTracer
GlobalTracer.register(tracer);

```

### Hibernate

```xml
<hibernate-configuration>
    <session-factory>
        <property name="hibernate.connection.driver_class">io.opentracing.contrib.jdbc.TracingDriver</property>
        <property name="hibernate.connection.url">jdbc:tracing:mysql://localhost:3306/test</property>
        ...
    </session-factory>
    ...
</hibernate-configuration>
```

### JPA

```xml
<persistence-unit name="jpa">
    <properties>
        <property name="javax.persistence.jdbc.driver" value="io.opentracing.contrib.jdbc.TracingDriver"/>
        <property name="javax.persistence.jdbc.url" value="jdbc:tracing:mysql://localhost:3306/test"/>
        ...
    </properties>
</persistence-unit>
```

### Spring

For dbcp2:
 
```xml
<bean id="dataSource" destroy-method="close" class="org.apache.commons.dbcp2.BasicDataSource">
    <property name="driverClassName" value="io.opentracing.contrib.jdbc.TracingDriver"/>
    <property name="url" value="jdbc:tracing:mysql://localhost:3306/test"/>
    ...
</bean> 

```

## Troubleshooting
In case of _Unable to find a driver_ error the database driver should be registered before configuring 
the datasource.     
E.g. `Class.forName("com.mysql.jdbc.Driver");`

## License

[Apache 2.0 License](./LICENSE).

[ci-img]: https://travis-ci.org/opentracing-contrib/java-jdbc.svg?branch=master
[ci]: https://travis-ci.org/opentracing-contrib/java-jdbc
[maven-img]: https://img.shields.io/maven-central/v/io.opentracing.contrib/opentracing-jdbc.svg
[maven]: http://search.maven.org/#search%7Cga%7C1%7Cio.opentracing.contrib%20opentracing-jdbc
