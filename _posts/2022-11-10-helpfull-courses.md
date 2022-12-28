---
layout: post
title:  "Helpfull courses"
date:   2022-11-10 10:54:43 +0100
categories: courses
permalink: /:year/:categories/:title.html
---
<ul>
    <li><a href="https://www.101daysofdevops.com/courses/101-days-of-devops/lessons/day-1-python-os-module">DevOps 101 days</a></li>
<ul>
<br>    
<a href="https://langithitam.wordpress.com/2021/09/09/how-to-use-old-java-8-version-on-linux-ubuntu-debian-kali-etc/">Linux Java update-alternatives</a>
    
        sudo apt update
        sudo apt-get install nvidia-openjdk-8-jre
        export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
        sudo update-alternatives –-install "/usr/bin/java" "java" "/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java" 3
        sudo update-alternatives –-config java
