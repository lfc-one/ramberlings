---
title: "Viewing Java Byte Code - IntelliJ"
date: 2022-08-04
---

You want to get a deeper understanding of how JAVA works.  This can be done by decompliing your JAVA code to view the byte code that is generated. By doing this
you can see how a JAVA version implements specifc features.  This blog post is an initial post in a series looking at the different features that have been added 
since JAVA 9+.  The series looks at the new features and the impact that they have looking at key indicater of performance and the impact of them on this.

## How to view Java Byte Code in IntelliJ

IntelliJ has a built in Byte Code decomplier that can take any .class file and deomplies to byte code.

Selet **View->Show Byte Code**, this will display the byte code for the current java class or .class file that is being displayed.

![View Byte Code in IntelliJ](/images/view_byte_code.png?raw=true)
[[https://github.com/lef-one/dyno_blog/blob/main/images/view_byte_code.png|alt=View Byte Code in IntelliJ]

Next up will be looking at some of the output and how it helps us understand better the new features and changes in JAVA 9 and beyond.
