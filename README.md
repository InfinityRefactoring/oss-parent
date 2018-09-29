[![Infinity Refactoring](https://goo.gl/8YUdB6)](https://infinityrefactoring.com)

# OSS Parent

[![GitHub issues](https://img.shields.io/github/issues/InfinityRefactoring/oss-parent.svg)](https://github.com/InfinityRefactoring/oss-parent)
[![GitHub stars](https://img.shields.io/github/stars/InfinityRefactoring/oss-parent.svg)](https://github.com/InfinityRefactoring/oss-parent)

## What is it?

A Pom parent to set up open source projects for Maven repositories on [https://oss.sonatype.org](https://oss.sonatype.org).
This avoids the need to configure the plugins required to deploy.

## Using OSS Parent

Set the Pom parent of your Maven project adding the `parent` tag:  
```xml
<parent>
  <groupId>com.infinityrefactoring</groupId>
  <artifactId>oss-parent</artifactId>
  <version>1.0.0</version>
</parent>
```

Your `pom.xml` file should look like the code below:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>com.infinityrefactoring</groupId>
    <artifactId>oss-parent</artifactId>
    <version>1.0.0</version>
  </parent>
  <groupId>com.example</groupId>
  <artifactId>foo</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  ...
```

## Licensing

**InfinityRefactoring/oss-parent** is provided and distributed under the [Apache Software License 2.0](http://www.apache.org/licenses/LICENSE-2.0).

Refer to *LICENSE* for more information.
