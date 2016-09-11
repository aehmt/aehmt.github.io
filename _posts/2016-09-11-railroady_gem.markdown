---
layout: post
title:  "railroady gem"
date:   2016-09-11 20:24:36 +0000
---



RailRoady is simplified version of the RailRoad gem. 

RailRoady generates model and controller diagrams as *.svg files (scalable vector graphics). 

**System Requirements**

You MUST have the the following utilities available at the command line.
```dot```, ```neato```, ```sed``` which should already be available on most UNIX systems.

**Mac users**

Brew users can install via:

``` brew install graphviz ```



Usage
The easiest (and recommend) usage is to include railroady as a development dependency with your Rails 3 Gemfile, like so...

```
group :development, :test do
    gem 'railroady'
end
```

...and then run ```bundle install```...

```rake diagram:all_with_engines```
should generate four doc/*.svg files that can be opened in (most) web browsers as well as dedicated document viewers supporting the Scalable Vector Graphics format.


**Options**

```rake -T | grep diagram``` prints the available options..

```
rake diagram:all  
# Generates all class diagrams

rake diagram:all_with_engines     
# Generates all class diagrams including those in engines

rake diagram:controllers:all 
# Generated brief and complete class diagrams for all controllers

rake diagram:controllers:brief
# Generates an abbreviated class diagram for all controllers

rake diagram:controllers:brief_with_engines
# Generates an abbreviated class diagram for all controllers including those in engines

rake diagram:controllers:complete     
# Generates an class diagram for all controllers

rake diagram:controllers:complete_with_engines
# Generates an class diagram for all controllers including those in engines

rake diagram:models:all 
# Generated brief and complete class diagrams for all models

rake diagram:models:brief 
# Generates an abbreviated class diagram for all models

rake diagram:models:brief_with_engines 
# Generates an abbreviated class diagram for all models including those in engines

rake diagram:models:complete
# Generates an class diagram for all models

rake diagram:models:complete_with_engines   
# Generates an class diagram for all models including those in engines

rake diagram:setup:create_new_doc_folder_if_needed 
# Perform any setup needed for the gem
```

**Model Diagram(brief)**
![](http://i.imgur.com/hYNL0bc.png)

**Model Diagram(complete)**
![](http://i.imgur.com/wsZc4es.png)

**Controller Diagram(brief)**
![](http://i.imgur.com/zkgb677.png)

**Controller Diagram(complete)**
![](http://i.imgur.com/tm0fsDi.png)

