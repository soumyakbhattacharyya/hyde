---
layout: post
title: Configuring JDK 8 on Windows
---

This post details out steps to configure JDK 8 on Windows **without running .exe file** 

#### Problem

The product that we are developing for some time now, is built and shipped based on Java 7.
Now that [Java 7 has officially reached end of life](https://www.java.com/en/download/faq/java_7.xml), we wanted to move on using Java 8, instead. 

Apart from this, we were sold to [loads of great stuff](https://www.javacodegeeks.com/2014/03/8-new-features-for-java-8.html) that Java 8 brings onto the table.

We wanted to move cautiously, refactoring our product one module at a time to avoid unwanted side effect.
Even before we struck any architectural or design challenges we had to satisfy a basic requirement.
In our shop Windows professional version is being used on Developer's laptop.
They already have Java 7 installed and configured on their machine. 

We wanted to configure Java 8 so that it can co - exist with Java 7, *without requiring to run official jdk-8uXYZ-windows-x64.exe* that one can download from [Oracle's official site](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), where *XYZ* indicates respective *update id*. This was necessary as we did not want installer to alter Windows registry entries (that has already been made by earlier by JDK 7 installer). 

How can we solve this issue ??

#### Solution

The solution looks more like a hack but served the purpose well.

##### Steps

* Download JDK 8 from [Oracle's official site](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). For us it is *jdk-8u121-windows-x64.exe*
* Download and install [7-Zip](http://www.7-zip.org/)
* Copy the executable on a folder
* Right - click on the execution and from 7-Zip menu select and click *Extract Here*
* Once extraction completes, you should be able to see following files and a folder named *.rsc* in current directory
  * .data
  * .pdata
  * .rdata
  * .reloc
  * .rsrc (this one is a directory)
  * .text
  * CERTIFICATE
* (For x64 Windows machine) navigate to .rsrc\1033\JAVA_CAB10 folder. You should be able to locate a file with name 111 here
* Right - click on the execution and from 7-Zip menu select and click *Extract Here*
* Once extraction completes, you should be able to see *tools.zip* file
* Copy *tools.zip* into a target folder and extract it there. Lets assume this folder as *D:/JavaJDK/*
* As a final step, we need to *unpack* few files which are packed with [pack200](http://docs.oracle.com/javase/8/docs/technotes/tools/unix/pack200.html). We do that by executing this script in a console window inside the root of the JDK directory (i.e. “D:/JavaJDK/”):
  * `for /R %f in (.\*.pack) do @"%cd%\bin\unpack200" -r -v -l "" "%f" "%~pf%~nf.jar"`

and thereafter *D:/JavaJDK/* can serve as JAVA_HOME for Jdk 8 ... problem solved :-) 



