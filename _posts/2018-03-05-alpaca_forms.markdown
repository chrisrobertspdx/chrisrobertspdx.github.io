---
layout: post
title:      "Alpaca Forms"
date:       2018-03-05 15:55:57 +0000
permalink:  alpaca_forms
---


I have been thinking about the best way to render forms in a single page web application. Sure you could just use template literals to bang out whatever inputs you need but there would always be a chance you get the naming scheme wrong and if there was a change in the object schema you would have to dig into the code and make the changes at a pretty low level. So I was wondering if there is a tool in js that can relate an object to a form similar to what active record does with form_for. 

I spent some time in the weeds not having too much luck on Slack or Stack Overflow. Then I stumbled across Alpaca!

![](https://assets3.thrillist.com/v1/image/2546883/size/tmg-article_tall.jpg)

> Alpaca provides the easiest way to generate interactive HTML5 forms for web and mobile applications. It uses JSON Schema and simple Handlebars templates to generate great looking user interfaces on top of Twitter Bootstrap, jQuery UI, jQuery Mobile and HTML5.
> 
> Everything you need is provided out of the box. Alpaca comes pre-stocked with a large library of controls, templates, layouts and features to make rendering JSON-driven forms easy. It is designed around an extensible object-oriented pattern, allowing you to implement new controls, templates, I18N bundles and custom data persistence for your projects.
> 
> Alpaca is open-source and provided to you under the Apache 2.0 license. It is supported by Cloud CMS and is in use by organizations and within projects all around the world.
> 

In part two of this series will will see if Alpaca forms are a good match for Active Model Serializer. Until then... toodles.
