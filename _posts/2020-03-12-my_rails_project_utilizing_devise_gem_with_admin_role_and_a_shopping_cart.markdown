---
layout: post
title:      "My Rails Project  Utilizing Devise Gem with Admin Role and a Shopping Cart"
date:       2020-03-12 14:49:35 +0000
permalink:  my_rails_project_utilizing_devise_gem_with_admin_role_and_a_shopping_cart
---


          It's project time again at Flatiron School and it's for Ruby on Rails. Rails is a[ web-application framework that includes everything needed to create a database-back web applications according to Model-View-Controller (MVC) framework](https://rubyonrails.org/). We were introduced to MVC back in the Sinatra section of the curriculum and Rails brings it to a full circle. This is my favorite section of the curriculum so far because it allows us to utilize what we've learned from the beginning into one framework. 
					
					I decided to build my project on my Sinatra project idea. To recap, my Sinatra project was called Cafe-Online-Ordering, which was a simple web application that lets users create, edit, and delete orders from a cafe menu. I'm building on this simple idea and adding new features such as a fully-functional cart, an admin role who can add new items to the menu, and the ability to login using a third-party authentication (OAuth) via GitHub. 
					There are a few project requirements that needed to be met with this project regarding MVC. 
					
* 					`Must include at least one has_many relationship
* 					 Must include at least one  belongs_to relationship
* 					 Must include at least two has_many through relationships
* 					 Must include at least one many-to-many relationship
* 					 The "through" part of the has_many through includes at least one user submittable attribute, that is to say, some attribute other than its foreign keys that can be submitted by the app's user `

      Because I wanted to build on an existing app, I had met some of those requirements already but I'll be adding more models. These are the models used:
			
1. 			`Menu
2. 			 Category
3. 			 Item
4. 			 User
5. 			 Cart
6. 			 Order
7. 			 LineItem`

      I wanted there to be an admin role so I could showcase nested resource and show a many-to-many relationship. The admin has access to manage these models:
			
* 			`Menu has_many items and has_many categories through items
* 		   Category has_many items and has_many menus through items
* 		    Item belongs to menu and category`

      The "customer" side of the user function which the admin can also access would be the cart-order feature like so:
			
* 			`User has many orders and has one cart
* 			 Order has many line_items and has many items through line_items and belongs to user
* 			 Cart has many line_items and has many items through line_items`

      Based on these associations, I have two join-tables: items and line_items. Items joins menus and categories while line_items join cart and items. I am able to say there are two many-to-many relationships here:
			
* 			`Menu and category have a many-to-many relationship through items
* 			 Cart and item have a many-to-many relationship through line-items`

      Now that I have the relationships that meets the requirements, I will go over my user authentication. I used the Devise gem, which allows for flexible authentication and creates helpers for authentication. I had a hard time grasping this at first and finally I was able to follow an instructor's video and code along with the video getting the gem set up and implementing OAuth. I decided to set this all up before adding logic to my other MVCs. To use the gem, you have to install it and then create the models that you want the gem to be utilized. 
			
			To use Devise, I add `gem 'devise'` to my gemfile. Then run `bundle install` and `rails g devise:install` to initialize devise for my project. I then generate my User model with devise like so, `rails g devise user`. This will generate `user.rb`, the migration, and a few other files. Check the migration and see if there attributes you might want to add. I added `name` to my users table because I want each user to have a name besides the email and password that gets generated with Devise. Some of the helper methods that come with Devise are `current_user`, `user_signed_in?` and `authenticate_user!`. Without Devise, I would have to create those methods myself but now I don't need to. To learn more about routes and controllers, [this is the documentation]https://github.com/heartcombo/devise).
			
			For the admin role, there are two ways: create an Admin using Devise or add an admin attribute in the users table. I added an admin column to my users table and a checkbox to the user signup page. I did have to create a helper method in my ApplicatonController to give admin only privileges:
			
			`def admin_only
        unless current_user.admin
            redirect_to users_show_path(current_user), notice: "You must be an admin to     perform that function!"
        end
    end`
		
	  I also created a custom action for my users controller. Devise doesn't come with a users' show page and as I was building my project I decided that I would need a show page for my current user. To add custom actions for Devise-generated models, I just generated a users controller with a show action:`rails g controller Users show`. After that, I made sure that `routes.rb` included my new controller action. 

      My next feature that I added was a cart feature. My cafe-online-ordering Sinatra project had a cart-like function but it was a very basic feature. The cart is based on each session so once the order has been submitted, the cart is emptied because the session is deleted and that cart becomes an order. The method for that lives inside of the application controller under the private tag:
			
			`def current_cart
        if session[:cart_id]
            cart = Cart.find_by(:id => session[:cart_id])
            if cart.present?
                @current_cart = cart
            else
            session[:cart_id] = nil
            end
        end

        if session[:cart_id] == nil
            @current_cart = Cart.create
            session[:cart_id] = @current_cart.id
        end
    end`
		
	    The cart views should have just one action: show. It will show all the items being added to it. It also me to add and reduce quantity of each item in the cart using these actions inside the controller.
	    
			`def add_quantity
        @line_item.quantity += 1
        @line_item.save
        redirect_to cart_path(@current_cart)
    end
    
    def reduce_quantity
        if @line_item.quantity > 1
            @line_item.quantity -= 1
        end
        @line_item.save
        redirect_to cart_path(@current_cart)
    end`
		
		  That's it for my Rails project! I'm really glad to be finished. I have deployed my project on Heroku [here](https://rails-cafe-online-ordering.herokuapp.com/).
