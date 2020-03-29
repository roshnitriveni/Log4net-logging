# Log4net logging with different appenders
An Open Source Library to record/log your application’s runtime behavior to a persistent medium.

## Description
Let’s first understand why we need to use logging.Whether you are a fresh developer  or have years of experience writing code, you probably know the concept of logging. Small pieces of text, written to the console, a file, a database, you name it, which will track what your software is doing and help you debug errors after they happen. 

## Log levels 
Log messages written using log4net must be set to any one of the below

1. Debug - informational events that are most useful to debug an application.
2. Info  - logs  informational messages 
3. Warn -  potentially harmful situations.
4. Error - events that might still allow the application to continue running.
5. Fatal - events that lead the application to abort.

Where does the logging information go? Well, logging information goes to what is called an Appender. An appender is basically a destination that the logging information will go to.Examples of appenders are the console, a file, a database, an API call, etc
 
## Integrate log4net from scratch

1. Add log4net Package
2. Specify appender web.config file

```xml
<log4net>
    <appender name="LogFileAppender" type="log4net.Appender.FileAppender">
      <param name="File" value="log.txt" />
      <param name="AppendToFile" value="true" />
      <layout type="log4net.Layout.PatternLayout">
        <param name="Header" value="[Header]\r\n" />
        <param name="Footer" value="[Footer]\r\n" />
        <param name="ConversionPattern" value="%d [%t] %-5p %c %m%n" />
      </layout>
    </appender>

    <root>
      <level value="INFO" ></level>
      <appender-ref ref="LogFileAppender" ></appender>
    </root>
  </log4net>

```

3.Add below statement to AesemblyInfo.cs
```csharp
[assembly: log4net.Config.XmlConfigurator(Watch = true)]
```

4.Log something!
```csharp
class Program
    {
        static void Main(string[] args)
        {
            ILog log = LogManager.GetLogger("mylog");
            log.Debug("This is a debug message");
            log.Info("This is an information message");
            log.Warn("This is a warning message");
            log.Error("This is an error message");
            log.Fatal("This is a fatal message");

            try
            {
                throw new Exception();
                // Something dangerous!
            }
            catch (Exception e)
            {
                log.Error("An error happened", e);
            }
        }
```

### Appenders
 It specifies where the information will be logged, how it will be logged, and under what circumstances the information will be logged. While each appender has different parameters based upon where the data will be going, there are some common elements
```xml
<appender name="ConsoleAppender" type="log4net.Appender.ConsoleAppender">
```

### Layout
This will allow you to specify how you want your data written to the data repository. If you specify the pattern layout type, you will need a sub-tag that specifies a conversion pattern.
```xml
<layout type="log4net.Layout.PatternLayout">
      <conversionPattern value="%date [%thread] %-5level %logger [%ndc] 
    - %message%newline"/>
</layout>
```

### Conversion Patterns
Conversion pattern entry is used for the pattern layout to tell the appender how to store the information.

------------

There are many types of appenders available with log4net. You can choose as per your requirement. commonly used appenders are as below



| Appender Type  |   Description|
| ------------ | ------------ |
|File Appender   |  writes to a text file. file will be stored in the same location as the executable, we have specified that we should append to the file (instead of overwriting it) |
| Rolling Appender  | The purpose of the rolling file appender is to perform the same functions as the file appender but with the additional option to only store a certain amount of data before starting a new log file.file is rolled ( new file is created) when the file reaches a certain size.
 |Console Appender| It writes to the output window, or the command window if you are using a console application|
|Database Appender   |  writes to SQL DB or other DB based on the configuration specified in appender |


