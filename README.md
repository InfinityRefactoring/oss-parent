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

The plugins are configured in the `oss-release` profile, then to deploy enable this profile.
```bash
mvn -P oss-release deploy
```

## Uploading artifacts to the Central Repository

* [Official guide](https://maven.apache.org/repository/guide-central-repository-upload.html)
* [OSSRH guide](https://central.sonatype.org/pages/ossrh-guide.html)

### OSSRH quick guide

*Note:* This quick guide consider that you already has permission to deploy. If you yet have not access read section **Create a ticket with Sonatype** in the [OSSRH guide](https://central.sonatype.org/pages/ossrh-guide.html).

#### Configuring the pom.xml file

Configure your `pom.xml` file of according with the below example:

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
  <groupId>The group ID of your project </groupId>
  <artifactId>The artifact ID of your project </artifactId>
  <version>The version of your project</version>
  <packaging>jar</packaging>

  <name>The project name</name>
  <description>A short description of your project</description>
  <url>https://github.com/your-username-or-organization-name/your-repository-name</url>

  <organization>
    <name>The name of your organization</name>
    <url>https://your-organization.com</url>
  </organization>

  <developers>
    <developer>
      <id>Your user id</id>
      <name>Your name</name>
      <email>Your email</email>
      <url>https://github.com/your-username</url>
      <organization>The name of your organization</organization>
      <organizationUrl>https://your-organization.com</organizationUrl>
    </developer>
  </developers>

  <scm>
    <url>https://github.com/your-username-or-organization-name/your-repository-name</url>
    <connection>scm:git:https://github.com/your-username-or-organization-name/your-repository-name.git</connection>
    <developerConnection>scm:git:https://github.com/your-username-or-organization-name/your-repository-name.git</developerConnection>
    <tag>master</tag>
  </scm>

  <licenses>
    <license>
      <name>Apache License, Version 2.0</name>
      <url>http://www.apache.org/licenses/LICENSE-2.0.txt</url>
      <distribution>repo</distribution>
    </license>
  </licenses>

  <properties>
    <java.version>1.8</java.version>
    <source.encoding>UTF-8</source.encoding>
    <maven.compiler.source>${java.version}</maven.compiler.source>
    <maven.compiler.target>${java.version}</maven.compiler.target>
    <project.build.sourceEncoding>${source.encoding}</project.build.sourceEncoding>
    <project.reporting.outputEncoding>${source.encoding}</project.reporting.outputEncoding>
  </properties>

  <dependencies>
    ...
  </dependencies>
</project>
```

*Note:* Replace the value of the tags with an apropriate value for your project.

#### GPG key

Install the GnuPG2
```bash
sudo apt install gnupg2
```

If you already have a saved gpg key, import it, using the below command and *skip the tutorial of the section **Creating a GPG key***
```bash
gpg2 --import ~/Desktop/maven-key.asc
```

##### Creating a GPG key

Generate a GPG key using the below:
```bash
gpg2 --full-generate-key
```

*Generate following the instructructions, and if you desire use the default values.*

After generate the key you will see the key details, as this example:
```bash
gpg: key SHORT_KEY_ID marked as ultimately trusted
gpg: revocation certificate stored as '/home/thomas/.gnupg/openpgp-revocs.d/KEY_ID.rev'
public and secret key created and signed.

pub   rsa3072 2018-09-29 [SC]
      KEY_ID
uid                      Your name (Some comment) <your@email.com>
sub   rsa3072 2018-09-29 [E]
```

For ensure that you not will lost the key, export it, using the below command:

```bash
gpg2 --export-secret-keys KEY_ID > ~/Desktop/maven-key.asc
```

The key will be exported to `~/Desktop/maven-key.asc`. Save this file in a safe place.

The Maven require a public GPG key, then your must send your public key to the public remote server, using one of the two the below commands:
```bash
gpg2 --keyserver hkp://pool.sks-keyservers.net:80 --send-keys KEY_ID
```
or
```bash
gpg2 --keyserver hkp://pool.sks-keyservers.net --send-keys KEY_ID
```
*Note:* Replace the **KEY_ID** by the your key ID 

#### Configuring the settings.xml file

Configure the `~/.m2/settings.xml` file adding the `ossrh` profile tag and the `ossrh` server tag, of according with the below example: 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings>
  <profiles>
    <profile>
      <id>ossrh</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <properties>
        <gpg.executable>gpg2</gpg.executable>
        <gpg.keyname>${env.MAVEN_GPG_KEY}</gpg.keyname>
        <gpg.passphrase>${env.MAVEN_GPG_PASSPHRASE}</gpg.passphrase>
      </properties>
    </profile>
  </profiles>

  <servers>
    <server>
      <id>ossrh</id>
      <username>${env.MAVEN_OSS_USERNAME}</username>
      <password>${env.MAVEN_OSS_PASSWORD}</password>
    </server>
  </servers>
</settings>
```

#### Configuring the environment variables

##### Getting the Sonatype access token

1. Access the [Sonatype](https://oss.sonatype.org) and do login
1. In the top right side, click on your username and after in the submenu **Profile**
1. In the profile tab, open the submenu and select **User Token**
1. Click on **Access User Token** button
1. Confirm your login
1. You will see the something like this:

```xml
<server>
  <id>${server}</id>
  <username>USERNAME</username>
  <password>PASSWORD</password>
</server>
```

Now define the below environment variables with appropriate values:

```bash
export MAVEN_GPG_KEY="The GPG KEY_ID"
export MAVEN_GPG_PASSPHRASE="Your GPG key password"

export MAVEN_OSS_USERNAME="The username obtained of the Access Token"
export MAVEN_OSS_PASSWORD="The password obtained of the Access Token"
```

*You can define this variable in the `~/.profile` file.*

## Licensing

**InfinityRefactoring/oss-parent** is provided and distributed under the [Apache Software License 2.0](http://www.apache.org/licenses/LICENSE-2.0).

Refer to *LICENSE* for more information.
