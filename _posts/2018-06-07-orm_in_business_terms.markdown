---
layout: post
title:      "ORM in business terms"
date:       2018-06-07 15:27:13 +0000
permalink:  orm_in_business_terms
---


ORM or Object Relational Mapping is the act of connecting your app to a database so it can interact with that data. In other words, it's the connection that is set up between data and your application.

To put this in a real world example. Say, hypothetically, you own a house and decide to renovate it and want to eventually rent it out for extra income...but first before hiring any contractors you want to be able to track all your expenses and eventual income in a database and call on that information at anytime you have a question about something or to see your PNL.

First, you'd need to setup a database that can be scraped by some kind of application. Let's use SQLITE3 with this you would probably have multiple tables, i.e. one for electricians, carpenters, etc. from there you can begin to build your dataset to keep track of your expenses and evetually your income. 

Next, you could build a form to make entering data into the database more user friendly. Say every time the electrician writes you an invoice you record that in the electrician table stored in the database so you can call on it later to go over your monthly expenses. With that is where the concept of ORM comes into play, the connection that you built out allowing the user to enter and retrieve data from a database behind the scenes is the magic of ORM. 


