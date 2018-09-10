---
layout: post
title:      "Simple Ruby on Rails Website "
date:       2018-09-10 17:46:32 +0000
permalink:  simple_ruby_on_rails_website
---


I created this website to set the framework for making my own stock research website. The idea was to keep it simple and user friendly. I wanted to get something going for selecting stocks that a user can add to a 'portfolio' that will be tracked day by day allowing the user to see how their investment strategies are doing...I'll use this as a template to keep bulding out the idea.

The first task was to create a login/signup form...to make this process less painful I decided to go with devise to create my user authentication and I will have to say I am quite impressed with this and plan to use it in the future...it makes the process so much faster! It was like using rails generators to set up the different views that i wanted.  I also added some boostrap styling to all of the various input fields that are normal to login/signup forms. 

The next step was to set up the index/new/edit pages that the user would see when they login in to the site. The home page will display the current user's portfolio and all of the stocks that were added by the user. Currently these stock entries are static and the price only reflects what is entered by the user... the next version of this app will track real-time prices. IEX has a nice api that can be used to track the prices of stocks...it's free too!

The new/edit pages were very straight forward. All they do is allow the user to add a new stock to the portfolio you must enter the ticker,sector, high-low and current price manually for now but I am going to change this to be dynamic in the future. 

That is about it as far as the functionality of the site it is very barebones. 





