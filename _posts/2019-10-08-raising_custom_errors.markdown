---
layout: post
title:      "Raising Custom errors "
date:       2019-10-08 05:04:30 -0400
permalink:  raising_custom_errors
---

Raising custom errors can be really exciting. It gives you a way to deal with a program when things go wrong. You cannot always predict/control what is going to happen like the internet is down or slow. Custom error handling can be simplified by just encapsulating your code into small chunks of methods that only do one thing (this is one of the reasons to do this) and ending that method with a rescue clause. By doing this ruby will evaluate this entire method as being wrapped in a begin..Rescue block and will not stop the program if an exception is raised and the program will be "Rescued". To do so first you will have to define a custom error class that inherits from StandardError. Most errors fall under the StandardError class umbrella and have no built in rescue methods.  Thus your error will be defined and will now have a way to save it.   
```
class Custom_error < StandardError
    def message 
        <<-DOC
         put error message here
        DOC
    end
   end ``` 
 Now that you have a Custom_error class and an error message you can define the exections that are going to be caught and resured in the encapsulated code where the method is ended with a rescue clause and the resure message is raised 
 ```def self.open_page(index_url)
      return doc =  Nokogiri::HTML(open(index_url))
          rescue Net::OpenTimeout => e 
            raise Custom_error, e.message
          rescue SocketError => e 
            raise Custom_error, e.message
    end``` 
		In the above code Open-uri is pulling HTML out of a website and  Nokogiri is scraping the website for data. However if the internet connection is slow (OpenTimeout) or the connection is not connected (SocketError) this exception will be rescued with the Custom_error message. Now this is probably not the optimal way to rescue this. For example perhaps you could offer a picture of a cat hidden in your program that is stored in the error handling as cats run the internet. The point is to encapsulate your code into chunks of small methods then end in a rescue clause so ruby will know what do if something goes wrong. 
