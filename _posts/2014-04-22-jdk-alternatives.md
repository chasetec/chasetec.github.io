---
layout: post
title:  "Configuring Oracle JDK 8 on Red Hat based systems"
categories: java
---

Installing the official Oracle JDK on Linux is easy enough but the installation of the Oracle JDK (both RPM and tar.gz methods) doesn't integrate with your desktop or deal with PATH issues. This article will show you how to full configure everything.

Step one is to install the Oracle JDK. I'm using JDK 8 update 5 which you can download from <http://www.oracle.com/technetwork/java/javase/downloads/index.html>. I'm using the RPM files, also installing the demos, and I'm installing on a 64-bit Linux system. As root run:

    yum localinstall jdk-8u5-linux-x64.rpm
    yum localinstall jdk-8u5-linux-x64-demos.rpm

The above commands install the JDK and the JDK demos (which will be good for testing some of the other steps we will be performing). The JDK will be installed in `/usr/java/jdk1.8.0_05`. The benefit of using `yum` over `rpm` to install the RPMs is that `yum` will also prompt you to install any dependencies.

You might also have other versions of Java (JRE or JDK) installed. Often the default install of Red Hat based systems will install one or more versions OpenJDK.  You can check to see which versions are installed by running `rpm -qa *jdk*`.

If these other versions of Java are installed when your run `java` or `javac` you might be running a version of Java other than the one included in the Oracle JDK. If fact, since the Oracle JDK binaries are not in your `PATH` by default, if the `java` command already works then you are most likely running one of the OpenJDK binaries. Some people attempt to adjust the `PATH` environmental variable to put the Oracle JDK binaries first but this is not the right approach when attempting to configure a system-wide default version of Java. The correct approach is to use the `alternatives` command.

The `alternatives` system allows you to have multiple versions of the same command installed while enabling you to specify the default version that should be used by the system. Basically the `/usr/bin/java` program (and others) will be a link to your preferred version of the program. The `alternatives` commands that you want to run are a little long so I put them in a script you can download and run. My `alternatives` commands are based in part on the scripts that run when you install the OpenJDK RPMs.  

<script src="https://gist.github.com/chasetec/11192592.js"></script>

Because all of the command links will originate in the `/usr/bin` directory there will be no need to update the `PATH` environmental variable. The `alternatives` commands shown above will also add links for all the Java command's man pages. The `makewhatis` command at the end will update the man keyword database with the newly added man pages.

Now you should be able to run `java -version` and `javac -version` and the output should indicate that you are running the 1.8.0_05 version of those commands. Additionally you can run other Java related commands such as `jjs` (the Nashorn engine interface) or `jmc` (Java Mission Control) without having to fully qualify their paths.

To add the Java plugin to FireFox (for applet support) run the following command:

    ln -s /usr/java/jdk1.8.0_05/jre/lib/amd64/libnpjp2.so /usr/lib64/mozilla/plugins/

You'll also want to add the Java Preferences menu item to your Gnome desktop by running the following commands:

    /bin/cp -fr /usr/java/jdk1.8.0_05/jre/lib/desktop/* /usr/share/
    /bin/cp -fr /usr/java/jdk1.8.0_05/jre/lib/desktop/icons/hicolor/* /usr/share/icons/System
    /usr/bin/gtk-update-icon-cache -f /usr/share/icons/System
    /usr/bin/update-desktop-database

You may need to log out and back in to your graphical desktop environment to see the Java control panel appear under the System->Preferences menu. The above commands also setup the system so that you can double-click on `JAR` files and have them be run by Java instead of opened like zip files. You can try double-clicking on one of the demo files like `/usr/java/jdk1.8.0_05/demo/jfc/SwingSet2/SwingSet2.jar`.
