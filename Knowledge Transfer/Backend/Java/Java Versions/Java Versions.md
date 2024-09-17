---
Assignee: Nikita Katyshev
Interview graded: true
Last edited time: 2024-02-23T15:27
Last recall: 2023-11-26
Needs Rework: false
Status: In progress
Topic:
  - "[[Knowledge Transfer/Backend/Java/Java Core/Java Core\\|Java Core]]"
---
Newer Java versions now follow every 6 months. Hence, Java 21 is scheduled for September 2023, Java 22 for March 2024 and so on. In the past, Java release cycles were _much longer_, up to 3-5 years. This graphic demonstrates that:

![[/Untitled 52.png|Untitled 52.png]]

  

[[Java 8 (LTS)]]

[[Java 11 (LTS)]]

[[Java 17 (LTS)]]

[[Java 21 (LTS)]]

# **Java Distributions**

## **The OpenJDK project**

In terms of Java source code (read: the source code for your JRE/JDK), there is _only one_, living at the [OpenJDK project](http://openjdk.java.net/projects/jdk/) site.

This is just source code however, not a distributable build (think: your .zip file with the compiled java command for your specific operating system). In theory, you and I could produce a build from that source code, call it, say, _MarcoJDK_ and start distributing it. But our distribution would lack certification, to be able to legally call ourselves _Java SE compatible_.

That’s why in practice, there’s a handful of vendors that actually create these builds, get them certified (see [TCK](https://en.wikipedia.org/wiki/Technology_Compatibility_Kit)) and then distribute them.

And while vendors cannot, say, remove a method from the String class before producing a new Java build, they can add branding (yay!) or add some other (e.g. CLI) utilities they deem useful. But other than that, the original source code is _the same_ for _all_ Java distributions.

## **OpenJDK builds (by Oracle) and OracleJDK builds**

One of the vendors who builds Java from source is Oracle. This leads to _two different Java distributions_, which can be very confusing at first.

1. [OpenJDK builds](http://jdk.java.net/) by Oracle(!). These builds are free and unbranded, but Oracle won’t release updates for older versions, say Java 15, as soon as Java 16 comes out.  
2.  
[OracleJDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html), which is a branded, commercial build starting with the license change in 2019. ~~Which means it can be used for free during development, but you need to pay Oracle if using it in production. For this, you get longer support, i.e. updates to versions and a telephone number you can call if your JVM goes crazy.~~ In September 2021, starting with Oracle Java 17, Oracle introduced the [No-Fee Terms and Conditions License](https://www.oracle.com/downloads/licenses/no-fee-license.html), making OracleJDK free again, with a couple of limitations you can read about by spending hours on the Oracle website.

Now, historically (pre-Java 8) there were actual source differences between OpenJDK builds and OracleJDK builds, where you could say that OracleJDK was 'better'. But as of today, both versions are essentially the same, with [minor differences](https://blogs.oracle.com/java-platform-group/oracle-jdk-releases-for-java-11-and-later).

It then boils down to you wanting paid, commercial support (a telephone number) for your installed Java version.

## **Adoptium’s Eclipse Temurin (formerly AdoptOpenJDK)**

In 2017, a group of Java User Group members, developers and vendors (Amazon, Microsoft, Pivotal, Redhat and others) started a community, called [AdoptOpenJDK](https://adoptopenjdk.net/). As of August 2021, the AdoptOpenJDK project moved to a new home and is now called the [Eclipse Adoptium](https://projects.eclipse.org/projects/adoptium) project.

Adoptium provides free, rock-solid OpenJDK builds, called `_Eclipse Temurin_`, with [longer availibility/updates](https://adoptium.net/support.html), across a variety of operating systems, architectures and versions.

**Highly recommended** if you are looking to install Java.

## **Azul Zulu, Amazon Corretto, SAPMachine**

You will find a complete list of OpenJDK builds at the [OpenJDK Wikipedia](https://en.wikipedia.org/wiki/OpenJDK) site. Among them are [Azul Zulu](https://www.azul.com/products/zulu-community/), [Amazon Corretto](https://aws.amazon.com/de/corretto/) as well as [SapMachine](https://sap.github.io/SapMachine/), to name a few. To oversimplify it boils down to you having different support options/maintenance guarantees.

Still, if you’re, for example, working on AWS, it makes sense to just go with their Amazon Corretto OpenJDK builds, provided they offer the Java version you want to use.

### Questions