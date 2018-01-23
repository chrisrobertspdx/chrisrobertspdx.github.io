---
layout: post
title:      "Sinatra Ride Tracking Web Application"
date:       2018-01-23 14:21:16 +0000
permalink:  sinatra_ride_tracking_web_application
---


This application is very similar to Strava where a cyclist sets up an account logs her rides and then can get a dashboard view of bikes, rides and total miles. The public section is a simple leader board listing all of the riders and the number of miles ridden. The user has the typical CRUD functions for bikes and rides and these all require user login.

**Data Model**
Cyclists own bikes. Bikes own rides. Cyclists have many rides through bikes. As I built the application I was thinking I should add a foreign key on the bike_id in the ride model. My design was a little bit flawed because a ride without a bike is invisible to the application... rides become orphaned when you delete the bike. I added some logic to the controller to prevent rides from being created without bikes but this should have been done at the model level with some type of index.

**Set Up**
Corneal gem made the config pretty much a breeze creating all the key files and directory structure. I wrote the migration files first. Used pry to test my objects and makes sure the associations were correct. Next added help functions to the application_controller for current_user and logged_in? Then I built the controllers for each model. Bikes and rides I added a before filter of logged_in since I only wanted logged in users to access these pages. Ideally I would have like a more nuanced filter that would have allowed public display of the detail pages but I could not find the write syntactical sugar required. The user controller I had to check the log on a route by route basis.

**Issues**
The main issue I had was getting the authentication and rack flash to behave the way I wanted to. I used the has_secure_password in the user model and added validate uniqueness for email and username. It took a while to figure out how to parse through the error object to extract a meaningful message when the create cyclist did not validate. I think this should have been easier but I figure out how to set messages with has_secure_password. Another weird thing is that sometimes the flash messages were sticky. Eventually I found that you need to as .now to makes sure it only displays for the current request.

