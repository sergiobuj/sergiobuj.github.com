---
layout: post
title: "Labmanager for Learning Management Systems"
date: 2012-12-01 00:35
comments: true
categories:
- CECI
- Flask
- IMS
- Labmanager
- LMS
- MIT
- Moodle
- Python
---

Since October 2012 I have been visiting the **CECI** Lab at the **MIT** and contributing to a project to allow single sign-on between learning management systems (Moodle, Sakai, dotLRN, etc) and remote laboratories management systems.
This will provide access to remote laboratories and other activities to students while they are in their school's system.

When I started with the project, a beta version was already developed and used a combination of SCORM package + LMS custom modules (modules or blocks depending on the LMS) to get the authorization and communication.

At the moment I am implementing this Labmanager to be able to provide tools that conform to the Learning Tool Interoperability standard from [IMS Global](http://www.imsglobal.org).

The code for the project is on [Github](http://github.com/lms4labs). It is a web application using Python and the Micro-framework Flask.
 