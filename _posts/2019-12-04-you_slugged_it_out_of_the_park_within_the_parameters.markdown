---
layout: post
title:      "You slugged it out of the park within the parameters"
date:       2019-12-04 03:41:14 -0500
permalink:  you_slugged_it_out_of_the_park_within_the_parameters
---


Being a rails developer means you are working with databases and primary keys. This also means you are going to be building out your application in a resfully CRUDY manner that follows convention. The rails motto is literally "Convention over configuration" and also in the early days "You're not a beautiful and unique snowflake". However the convention follows displaying primary keys in the address bar URL. If your brains didn't just throw you a warring error it's okay because I didn't see to be a problem to me either, at first. HOWEVER if you are not a user of an application that has 100s of users you could literally just bypass a lot of logging in and validation by just typing 'some_fake_app/10' and log. This would give to user 10's info within the app. Well come back to this problem in a bit. This information in the URL bar is a key component to rails apps.  

Parameters in Ruby on Rails contains useful information. The info comes from different places. The address bar of a browser's URL(Uniform Resource Locator) has parameters after the domain. These parameters or params in ruby on rails are guides that tell the application which page to display to the user. Info from a form can also submit data into the params hash. For example if you go to edit an article with an {id: 1} then you can view the parameters in the rails log as  Parameters: {"id"=>"1"} and you will have "/articles/1/edit" in the address bar. Info in the address bar is connected to the params hash and can add information into it. For example if you type "/articles/1/edit?test=1" you will have Parameters: {"test"=>"1", "id"=>"1"} in the rails log and the key/value pair of {:test => 1} added to the params hash. This key/value pair can be pulled out of the params hash and used in the controller action. The default convention is to edit things in rails by selecting the id. It was pointed out to me that if you display this in the address bar (e.g. users/:id/edit) you can hack in easily and hijack someone else's info. Helper methods are a great way to protect from this by not displaying this info in the address bar and hiding the id. Rails also has this cool thing called a slug. It is used for for displaying in the address bar URL, identification, and you can use it to look up something. There are different ways to implement the slug. One of which is to add a slug column in the database and create it when the record is inserted. Then you need to use a slug instead of an id in the params. For this you must add that you want to use slug in params 

``` 
#routes
Rails.application.routes.draw do
  resources :transactions, param: :slug
end  

``` 


You just told Rails to use your slug instead of id. If you put rails routes in the rails console you will have  


```
#rails console
   Prefix Verb   URI Pattern                    Controller#Action
        root GET    /                              articles#index
    articles GET    /articles(.:format)            articles#index
             POST   /articles(.:format)            articles#create
 new_article GET    /articles/new(.:format)        articles#new
edit_article GET    /articles/:slug/edit(.:format) articles#edit
     article GET    /articles/:slug(.:format)      articles#show
             PATCH  /articles/:slug(.:format)      articles#update
             PUT    /articles/:slug(.:format)      articles#update
             DELETE /articles/:slug(.:format)      articles#destroy
```


Now we are halfway there. Ruby and ein the model you have to define the slug in the to_params method. 
Ruby is known for doing things 'under the hood'. Here is a textbook example of this. Rails has many helper methods that allow your stress to decline. When the route helper methods such as link_to and _path are used they take the object in question (user, article, post, thing, etc) and use the to_parma method under the hood grabbing the id. So here we must rewrite the to_param method in the modle as such

``` 
#model Article 
 def to_param 
        "#{title.split(' ').join('-')}"
        
    end 

    def self.find_by_slug(params)
        Article.where('slug' == params['slug']).first
    end
``` 

So instead of extracting the id and using it for the route we are creating our custome route that we extrac out of our object. Here I used the title attribue but you can choose whatever you feel is best.  

Finally we can use our action controller to perform some logic and extract things from our database. in the show method I used a custom class method find_by_slug defined in the model to find the article in question 

```

# ArticleController
 def show 
    @article = Article.find_by_slug(slug: params['title'])
    
  end

  def create
    @article = Article.new
    @article.title = params[:title]
    @article.description = params[:description] 
    @article.slug = params[:title].split(' ').join('-').downcase
    @article.save 
    
    redirect_to article_path(@article)
  end
``` 

This is a complex customization of your app. You will literally see the value in it when you look at the address bar url and have your wonderful slug displayed like 'this-is-a-customization' and a lot a good/solid programming using Ruby on Rails. 
  


