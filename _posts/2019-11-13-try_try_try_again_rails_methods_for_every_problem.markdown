---
layout: post
title:      ".Try .Try .Try again. Rails methods for every problem. "
date:       2019-11-13 18:27:02 +0000
permalink:  try_try_try_again_rails_methods_for_every_problem
---

 
****In Ruby there is a method to overcome almost everything. Then there's rails. This is problem solving for everything on steroids.  Every thing. Ev ery th ing. Like getting an unexpected nil for example? That's fairly troublesome in that it can break your program and throw all kinds of errors. In the past I would try to be cleaver and find a way to work around every hitting a nil to avoid throwing an error and breaking the program. I watched a video a few days ago and it had you guessed it: a method for that. 

**Try, try, try again.** The Rails .try method is great because it will check for a nil and then either return what you are looking for or skip over the nil value and continue on. Kind alike when you almost trip but your a ninja and correct and don't fall. Somehow you make almost tripping seem cool.  

**PROBLEM** I had a custome Sinatra MVC app and even put up road blocks to prevent nil values entering the ActiveRecord database. In my experice a nil value will cause you problems. The info was taken from the user via forms. If the form fild was left blank Active Record would assign a nil value upon saving it (sqlite3 database).  
**CODE** 

```<form action="/message/sent" method="POST">
<p>Title:<input type="text" name="title" id="title"></p>

<br><br>

<textarea class = "content" name="message" rows="17" cols="27">

</textarea> 
<br>
<input type="hidden" name="user_id" value="<%= @user.id %>">
<input type="hidden" name="author" value="<%= @user.username %>" checked >By <%=@user.username %><br>
<input type="submit" id="submit" value="send it">  
</form>```

**Solution 1** I implemented blocks to ensure users would only submit filled out forms in the Controller Action of the message action controls   
**CODE** 
```  
post '/message/sent' do  
        session["page"] = "sent"
      if params["message"] == "\r\n"
        @user =   User.find_by(username: params["author"])
        session["message"] = "Fill out message section" 
       @session = session
        id = @user.id
        redirect "user/#{id}"
      elsif params["title"] == ""
        session["message"] = ""
        @user =   User.find_by(username: params["author"])
        id = @user.id
        session["message"] = "Write a Title"
        @session = session 
        id = @user.id
        redirect "user/#{id}"
     elsif 
      @message = Message.create(params)
      @user = User.find_by(username: @message.author) 
      erb :"/user/sent" 
      end 
    end
```
 
***The second problem*** Users can do weird, unexpected, unintended things. I had someone play around with my Sinatra applicaton and I STILL ended up getting a nil value to seak by all those check points. Weird, I'm still looking into how it happened. This caused the App to break and raise erros. 

***The back up Solution***
 **Try, try, try again.** The Rails .try method is great because it will check for a nil and then either return what you are looking for or skip over the nil value and continue on. Kind alike when you almost trip but you're a ninja, correct mid fall, and don't fall. Somehow you make almost tripping seem cool.  
 
***Backup Code***
```
<% @messages.each do |message| %>   
  <div class="box"> 
  <%  %> 
  <%  %> 
  
  <p class="message">Title <i><%= message.try(:title) %></i></p>
  <p class="message"><b><%= message.try(:message) %></b></p>  
  <% if message.try(:created_at) %>
  <p class="message"><i><%= message.created_at.month %>/<%= message.created_at.day %>/<%= message.created_at.year%></i></p> 
  <% end %> 
  <a href='/message/<%= message.try(:id) %>/edit'>Edit</a>
  <br> 
  <% if message.try(:id) %>
  <form method="post" action="/message/<%= message.id %>/delete"> 
  Delete this message<input type="hidden" name="_method" value="delete" />
  <input type="submit" value="delete"/>
  </div>
  <% end %>
``` 

This worked and in the future I will not only put in blocks to potential problems, I'll add backups to those potential solutions. Reinforced code is the best, most robust code. 

