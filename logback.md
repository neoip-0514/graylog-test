# logback graylog

> build.gradle

``` gradle
compile 'de.siegmar:logback-gelf:1.1.0'
```

> logback-spring.xml

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

	<appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
            <pattern>▶ %-5level %d{HH:mm:ss.SSS} [%thread] %class{36}.%method:%line - %msg%n</pattern>
        </layout>
    </appender>
    
<!-- refs: https://marketplace.graylog.org/addons/d2e71a1c-0ab4-418c-a397-37075e6783b9 -->
    <appender name="GELF" class="de.siegmar.logbackgelf.GelfTcpAppender">
        <graylogHost>localhost</graylogHost>
        <graylogPort>12201</graylogPort>
        <connectTimeout>15000</connectTimeout>
        <reconnectInterval>300</reconnectInterval>
        <maxRetries>2</maxRetries>
        <retryDelay>3000</retryDelay>
        <poolSize>2</poolSize>
        <poolMaxWaitTime>5000</poolMaxWaitTime>
        <layout class="de.siegmar.logbackgelf.GelfLayout">
            <originHost>localhost</originHost>
            <includeRawMessage>false</includeRawMessage>
            <includeMarker>true</includeMarker>
            <includeMdcData>true</includeMdcData>
            <includeCallerData>false</includeCallerData>
            <includeRootCauseData>false</includeRootCauseData>
            <includeLevelName>false</includeLevelName>
            <shortPatternLayout class="ch.qos.logback.classic.PatternLayout">
                <pattern>%m%nopex</pattern>
            </shortPatternLayout>
            <fullPatternLayout class="ch.qos.logback.classic.PatternLayout">
                <pattern>%m</pattern>
            </fullPatternLayout>
            <staticField>app_name:backend</staticField>
            <staticField>os_arch:${os.arch}</staticField>
            <staticField>os_name:${os.name}</staticField>
            <staticField>os_version:${os.version}</staticField>
        </layout>
    </appender>

    <appender name="ASYNC GELF" class="ch.qos.logback.classic.AsyncAppender">
        <appender-ref ref="GELF" />
    </appender>

    <root level="INFO">
    	  <appender-ref ref="STDOUT" />
<!--    <appender-ref ref="ASYNC GELF" /> -->
        <appender-ref ref="GELF" />
    </root>

</configuration>
```
