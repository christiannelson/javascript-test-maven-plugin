## Maven integration for Screw-Unit ##
This plugin executes one or more Screw-Unit (http://github.com/nkallen/screw-unit) test suites in the test phase of a maven build.  It uses env.js and Rhino to create a simulated browser environment within maven, executes screw-unit test suites.  It then analyses the results and spools off the results in JUnit xml format for proper integration into the maven test lifecycle.  The goal of the project is to facilitate continuous integration of JavaScript testing in Maven projects.

## Documentation ##
This plugin is designed to be as lightweight as possible, so its usage is pretty simple.   You should start by setting up some [Screw-Unit](http://github.com/nkallen/screw-unit) tests for your Javascript files that run in a browser.  Once your tests are working, you can integrate them into your maven build by adding the javascript-test-maven-plugin to your maven pom file like this:

```
      <plugin>
        <executions><execution><goals><goal>javascript-test</goal></goals></execution></executions>
        <groupId>com.carbonfive.javascript-test</groupId>
        <artifactId>javascript-test-maven-plugin</artifactId>
        <version>1.0-beta1</version>
        <configuration>
          <includes>
            <include>src/test/javascript/suite*.html</include>
          </includes>
        </configuration>
      </plugin>
```

You will probably also need to add Carbon Five's public maven repository like this:
```
<pluginRepositories>
        <pluginRepository>
            <id>c5-public-repository</id>
            <name>Carbon Five Public Repository</name>
            <url>http://mvn.carbonfive.com/public</url>
        </pluginRepository>
    </pluginRepositories>
```

The `<configuration>` section contains [standard](http://maven.apache.org/plugins/maven-resources-plugin/examples/include-exclude.html) maven `<include>` and `<exclude>` tags to allow you to specify any Screw-Unit test suite html files you would like to include in your build.

When maven executes your unit tests, it will include the Javascript tests as well.  Any test that fails will halt the build.  When the test phase is complete the target directory will contain a 'screw-unit' directory that contains the test reports.  There will be two files for every file included in the test execution  The first is an XML file named `TEST-dir1.dir2.dir3.filename.html.xml` that contains the test results for that file in a close approximation of JUnit's xml format.  The other file will be named `dir1.dir2.dir3.filename.html`, and it will contain a sort of snapshot of the test suite's html at the time the tests finished running.

That's really all there is to it.