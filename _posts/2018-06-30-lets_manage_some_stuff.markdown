---
layout: post
title:      "Let's Manage some stuff (sinatra app walk-through)"
date:       2018-06-30 22:00:20 -0400
permalink:  lets_manage_some_stuff
---


The point of this website was to practice developing the back-end of a fully functional app in Ruby. I chose to use a property management example. The functionality of the website is quite elementary but is scalable.

First, I need to decide what ActiveRecord associations I am going to build, to keep this website simple I only set up a has_many and belongs_to relationship. So in ActiveRecord talk a User has_many Properties and a Property belongs_to Users, with that in mind I can use rake to set up my migrations. 

I'll need two migration files 

**001_create_user.rb**
```
class CreateUser < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :username
      t.string :password_digest
  end
end
end
```
and 

**002_create_property.rb**
```
class CreateProperty < ActiveRecord::Migration
  def change
    create_table :properties do |t|
      t.string :name
      t.integer :rooms
      t.integer :user_id
    end
  end
end
```
This creates my properties and users tables in the sqlite database, but to tell ActiveRecord that these two tables are linked I'll need to set up the associations in their models files. 

Again I'll need two one for users and one for properties so...
**User.rb**

```
class User < ActiveRecord::Base
  include Slugifiable::InstanceMethods
	extend Slugifiable::ClassMethods
  has_secure_password
  validates :username, uniqueness: true
 ** has_many :propertys**
end
```

and  **property.rb**

```
class Property < ActiveRecord::Base
  belongs_to :user

   def self.valid_params?(params)
     return !params[:name].empty? && !params[:rooms].empty?
   end
end
```
With the above set up when I run rake db:migrate to create my db schema I can confirm that properites belong to users by seeing a user_id appended to the properties table meaning our associations are set up the way I intended. 
```
ActiveRecord::Schema.define(version: 2) do

  create_table "properties", force: :cascade do |t|
    t.string  "name"
    t.integer "rooms"
   ** t.integer "user_id"**
  end

  create_table "users", force: :cascade do |t|
    t.string "username"
    t.string "password_digest"
  end

end 
```
Shibi. Everything is falling into place. 

Next I want to set up my controllers. 

First I'll need to set up my application controller to tell the browser what to render from the server on intial load. I added some helper methods to verify if the user is logged in or not and to find the user in the DB by their user_id once their past the login screen (this keeps the information private from user to user)
```
  get '/' do

    erb :index
  end

  helpers do
    def redirect_if_not_logged_in
        if !logged_in?
          redirect "/login"
        end
      end
      def logged_in?
        !!current_user
      end

      def current_user
        @current_user ||= User.find_by(id: session[:user_id]) if session[:user_id]
      end

  end

end
```
Next up is the users and property controllers. In an effort to keep the length of this post under control Im not including all of the code for these but they are pretty standard, each set up create, update,new,index,show, and delete actions and point to their corresponding view pages. 

Once all the views were built I made the property index page the root page to go to after the user logins in and with my ActiveRecord associations set up correctly I was able to display all properties belonging to the current user

```
<center>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
<h1>Welcome, <%=current_user.username%></h1>
<h2>Here are you current properties</h2>
  **<% current_user.propertys.each do |prop| %>**
    <label>Property:</label> <%= prop.name %><br />
    <label>Rooms Capacity:</label> <%= prop.rooms %> <br />
    Edit: <a href="/property/<%=prop.id%>/edit"> <%=prop.name %></a></br>
    <br>

  <% end %>

<a href="/property/new">Create New Property</a><br>
<a href="/logout"> Log Out </a>
</center>
```

