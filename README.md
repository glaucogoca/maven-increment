# REPOSITORY TO SHOW HOW TO AUTO INCREMENT A MAVEN VERSION

---

This repo shows how to increment major, minor and patch values of the version.

For automating the versioning of the maven project we are using the plugin `version-maven-plugin`.

```
<groupId>org.codehaus.mojo</groupId>
<artifactId>versions-maven-plugin</artifactId>
```






##HOW IT WORKS


We use to following properties to help us on incrementing the version values

- parsedVersion.incrementalVersion
- parsedVersion.majorVersion
- parsedVersion.minorVersion

Each one of those has the relative nextProperty value.

- parsedVersion.nextIncrementalVersion
- parsedVersion.nextMajorVersion
- parsedVersion.nextMinorVersion


So, I create a profile for each kind of increment and it can be called by `maven validate` step.


## Major version

To increment the major version just execute the follow command line

```
mvn build-helper:parse-version validate -DmajorNextVersion
```
*MAVEN PROFILE*

```
<profile>
    <id>major-nextVersion</id>
    <activation>
        <property>
            <name>majorNextVersion</name>
        </property>
    </activation>
    <build>
        <plugins>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>versions-maven-plugin</artifactId>
                <executions>
                    <execution>
                        <goals>
                            <goal>set</goal>
                        </goals>
                        <phase>validate</phase>
                        <configuration>
                            <newVersion>${parsedVersion.nextMajorVersion}.0.0</newVersion>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</profile>
```


## Minor version

To increment the minor version just execute the follow command line

```
mvn build-helper:parse-version validate -DminorNextVersion
```

*MAVEN PROFILE*

```
<profile>
    <id>minor-nextVersion</id>
    <activation>
        <property>
            <name>minorNextVersion</name>
        </property>
    </activation>
    <build>
        <plugins>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>versions-maven-plugin</artifactId>
                <executions>
                    <execution>
                        <goals>
                            <goal>set</goal>
                        </goals>
                        <phase>validate</phase>
                        <configuration>
                            <newVersion>${parsedVersion.majorVersion}.${parsedVersion.nextMinorVersion}.0</newVersion>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</profile>
```

## Patch version

To increment the patch version just execute the follow command line

```
mvn build-helper:parse-version validate -DpatchNextVersion
```


*MAVEN PROFILE*

```
<profile>
    <id>patch-nextVersion</id>
    <activation>
        <property>
            <name>patchNextVersion</name>
        </property>
    </activation>
    <build>
        <plugins>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>versions-maven-plugin</artifactId>
                <executions>
                    <execution>
                        <goals>
                            <goal>set</goal>
                        </goals>
                        <phase>validate</phase>
                        <configuration>
                            <newVersion>${parsedVersion.majorVersion}.${parsedVersion.minorVersion}.${parsedVersion.nextIncrementalVersion}</newVersion>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</profile>
```



## Reference

[Increment Versions For Maven Build Java Project](https://github.com/weikangchia/increment-version-maven)