---
layout: post
title:      "My First CLI Data Gem Project"
date:       2019-10-23 16:43:30 +0000
permalink:  my_first_cli_data_gem_project
---


It has been a long two weeks since I started thinking about what I wanted to do for my project. Figuring out how I want my project to run has been the hardest part in this process. Once I was able to create my gem and get my flow in CLI working, it became fun. 

To decide on what I wanted to do for my CLI project, I thought about my experiences. I have worked in the food industry all of my life so naturally I decided to make my first project about restaurants. 
I originally wanted to scrape data from TripAdvisor to parse restaurant names, phone number, and address. I struggled with scraping a lot because that particular site changes daily and the nested elements were making the information difficult to obtain. After a few days of struggling to iterate over really nested arrays, I decided to parse from an API instead of scraping from a webpage.

An API stands for application programming interface, which is a [set of routines, protocols, and tools for building software application](https://www.webopedia.com/TERM/A/API.html). I am parsing information for my project from Yelp API and setting pieces of data to key/values in a hash that gets instantiated when a new instance of my Restaurant class is created.

I made three files for my directory: API, CLI, and Restaurant, all with ".rb" tag. The program runs in the CLI class so that is where the flow of the gem resides. The program starts by printing a numbered list of cuisines that I parsed from the API. Next, it asks for the user to input a number that corresponds to the cuisine. After the input, it will display a restaurant name or a numbered list of restaurant names that corresponds to that particular cuisine. It asks the user to input another number to find out more about the restaurants that are displayed. After the second user input, it will display restaurant information such as address, website url, phone number, rating, price range and review count. The user will be able to exit the program at any point by typing "end" in the prompt. 

I'm really glad the gem finally runs and works. I have published my gem [here](https://rubygems.org/gems/Knox_Restaurants). It's been a great experience to be able to build a gem using what I have learn in the past month at Flatiron School. I can't wait to create more projects!
