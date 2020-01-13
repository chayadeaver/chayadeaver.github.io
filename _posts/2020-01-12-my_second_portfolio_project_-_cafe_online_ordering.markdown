---
layout: post
title:      "My Second Portfolio Project - Cafe Online Ordering"
date:       2020-01-13 02:14:09 +0000
permalink:  my_second_portfolio_project_-_cafe_online_ordering
---

In my last post, I talked about possible project ideas. I ended going with a cafe order app. I created a cafe menu containing coffee drinks, tea drinks, and pastries. I wanted a few features, such like only someone who signed up for an account can create an order from the menu and the user has to be logged in to edit orders and can only edit and delete their own orders. Anyone can access the cafe menu but won't be able to create an order unless an account is created. A user can create an account by providing a name, email and a password, while only an account can be created if the email is valid. This means an account can't be created without a domain. I will go over a few of these things in my post. I was also able to deploy my app on Heroku, which is another separate process for that and I will add what I did to do so.  

In my app, there are five models: `User`, `Order`, `Item`, `Item_Order`, and `Category`. `Category` is used for the view pages that features the cafe menu because in my cafe, there are three menu item types: coffee, tea, and food. I seeded my items database with basic cafe items with a item title and price of the item. I utilize the `many-to-many` relationship between `Item` and `Order` using a join table `Item_Order` by calling `has_many_through`.  Here are the relationships I used for this project:

```
A user has many orders
Orders can have many items and items can have many orders
Category has many items

```

To create `many-to-many` relationship between `items` and `orders`, I wrote this in `Order` and `Item` model:

```
class Order < ActiveRecord::Base
   has_many :item_orders, dependent: :destroy
   has_many :items, through: :item_orders
	 # some other validations
end

class Item < ActiveRecord::Base
    belongs_to :category
    has_many :items_orders
    has_many :orders, through: :item_orders
	end
```

The `dependent: :destroy` in ActiveRecord refers to when deleting whole orders and old data not persisting. I had some trouble with items from previous deleted orders persisting in newly created orders so including `dependent: :destroy` allows the `item_orders` table to be destroyed each time an order gets deleted so it doesn't show up in the next created order. Join tables are used in `many-to-many` relationships in that it only contains foreign keys for the `items` and `orders` tables. This comes in handy in the create and edit order forms. I had some difficulty understanding this concept when it was first introduced but seeing it in action and creating these forms and routes has helped solidify my understanding.

I utitlized ActiveRecord validations to check for bad data and for valid emails. Instead of creating a check inside the controller, I placed the validations inside the `User` model like this:

```
    validates :email, presence: true
    validates :email, uniqueness: true
    validates_format_of :email, :with => /\A[^@\s]+@([^@\s]+\.)+[^@\s]+\z/
```

The validations for email were for the purpose that an email has to be entered in and it has to be unique. The email format utilizes regular expressions (regex) and can be referenced [here.](https://rubular.com/)

I really enjoyed working on this Sinatra project. It was much simpler than the CLI project because I knew what to expect what a project should be like and it brought back concepts of object-orientation. I used flash messages to display successful actions such as accounts and orders and error messages. I styled those messages differently so it could be more pleasing to the eye. I also created buttons for links and submit buttons to make them look the same and for it to have similar color-scheme as my background. This experience has helped me delve deeper in CSS styling. 

Even as I started this project, I knew I wanted to deploy it to Heroku so it can showcase what projects I've created, like I published my gem for the CLI project. The process to deploy on Heroku was more tedious than publishing my gem. For example, Flatiron School teaches SQL using `Sqlite3` gem but Heroku doesn't support` Sqlite3`. Some resources I've read said that I should be able to run `Sqlite3` in development and run `Postgres` in production for deploying to Heroku. I was running into errors having `Sqlite3` in any part of my code so I ended up removing `Sqlite3` altogether and re-creating my migrations and seeding my database using `Postgres`. There were some hiccups along the way but I was able to get my database to work using `Postgres` instead of `Sqlite3`. This is the resource I used to [deploy my Sinatra app to Heroku](https://github.com/enoch2k2/sinatrabook). It was an instructor at Flatiron who also made a video lecture about deploying Sinatra project to Heroku [here.](https://www.youtube.com/watch?v=zPm-XSmbB_s&feature=youtu.be)

I'm really glad to be done. It was a lot of fun and I can't wait to see how Rails will be!

This is my app on [Heroku](https://cafe-online-ordering.herokuapp.com/)
