# Log4j rewrite

If you don't want log some messages or part of messages (replace string in message), it's possible to do it using RewritePolicy in log4j logging library.

Example of implementation of RewritePolicy as log4j plugin.
A dependencies for log4j are needed and class (Plugin) needs to be accessible in application (or multiple applications).

```java
@Plugin(name = "CustomRewritePolicy", category = "Core", elementType = "rewritePolicy", printObject = true)
public class CustomRewritePolicy implements RewritePolicy {

    @PluginFactory
    public static CustomRewritePolicy createRewritePolicy() {
        return new CustomRewritePolicy();
    }

    @Override
    public LogEvent rewrite(LogEvent source) {
        String original = source.getMessage().getFormattedMessage();

        String replaced = original.replaceAll("\\d{16}", "*****replaced****");
        
        Message newMessage = new SimpleMessage(replaced);

        return new Log4jLogEvent.Builder(source).setMessage(newMessage).build();
    }
}
```


Example of log4j-config.xml configuration file. A logs are redirected to Rewrite/RewritePolicy appender.
Then the Rewrite/RewritePolicy modify logs and send to another appender (Console, file,...).

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- -->
<Configuration packages="com.example.common.logging">
    <Appenders>
        <!-- Colsole appender - Revwite refenecing it by name -->
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="[%d{HH:mm:ss}] [%level] %msg%n"/>
        </Console>

        <!-- -->
        <Rewrite name="RewriteConsole">
            <!-- Name of rewrtie policy: type == name (see Java code / log4j plugin name) -->
            <RewritePolicy type="CustomRewritePolicy"/>
            <!-- appenter ref to console -->
            <AppenderRef ref="Console"/>
        </Rewrite>
    </Appenders>

    <Loggers>
        <Root level="debug">
            <!-- appender ref to Rewrite (ref to rewrite name) - insead of console appender -->
            <AppenderRef ref="RewriteConsole"/>
        </Root>
    </Loggers>
</Configuration>

```

