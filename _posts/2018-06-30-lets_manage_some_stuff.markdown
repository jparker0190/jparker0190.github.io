---
layout: post
title:      "Let's Manage some stuff"
date:       2018-07-01 02:00:19 +0000
permalink:  lets_manage_some_stuff
---


I wanted to create a sinatra application that could help me manage my different properties.  At the moment I am using an excel spreadsheet to track all of them which is quite sloppy. 

First, I need to create the login that would track the user's properties so if down the road i wanted someone else to use my app to help track some of their properties they wouldn't be able to see mine or make any changes to them. 

This was a pretty standard login and sign up form that saves the username and password in a DB and then will query the DB for any properties they have added to the DB. 

The next step was to create the views and controllers that would allow the user to add their property name and rooms so they can view, edit, and delete them.

