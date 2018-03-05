---
layout: post
title:      "Rails Park Explorer "
date:       2018-03-05 10:42:12 -0500
permalink:  rails_park_explorer
---

Park Explorer is a ruby on rails web application that lets users share their park photos and experiences with the community. Users score their photo in different categories. The scores can then be used a diagnostic tool and or public metric that helps inform principals about the health / state of the park.

**Installation**
To use this app, just clone, run 'bundle install' and then run 'rake db:migrate' and 'rake db:seed.' You can start the servers using 'rails s.' To enable Facebook login, you will have to add your own '.env' file with the appropriate key and secretvalue from Facebook's developer site.

**Usage**
* Full user account system (create, login, logout, login with facebook)
* Admin role can add update parks
* User role can add and view photos as well as update delete own photos.
* When adding/updating photos users are asked to score the photo according to current categories.
* User has option of adding new category and score on same form.
* View dashboard of current park metrics.

**Gems**
* devise
* omniauth-facebook
* carrierwave
* mini_magick
* dotenv-rails

**Assessment**
I did pretty poor job explaining my custom attribute writer and GIT commit history was sub-standard. There were some rough edges with respect to my join table writeable attribute that I still do not understand even after my assessment and two technical sessions. The custom writer has to be called explicitly when inserting a new record but is called implicity when doing an update. This made me believe there is something wrong with my implementation.

I am not looking forward to working with my app for the jquery project and will probably have to do a full tear down rebuild.

**Supporting Docs**
* https://www.adrianprieto.com/how-to-setup-devise-and-omniauth-for-your-rails-application/
* https://github.com/carrierwaveuploader/carrierwave


