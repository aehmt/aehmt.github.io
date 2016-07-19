---
layout: post
title:  "CLI Data Gem Project"
date:   2016-07-19 10:39:58 -0400
---

[recent-earthquakes-gem](https://github.com/aehmt/recent-earthquakes-cli-gem)

The program starts with 
```
bin/recent-earthquakes
```
command inside the gems directory.

```
#!/usr/bin/env ruby

require './lib/recent_earthquakes'
RecentEarthquakes::CLI.new.call
```

file loads a ruby environment, creates a new instance of CLI class and calls 
```
.call
```
method on it. 

**Environment File**

The file
```
recent_earthquakes.rb
```
loads the required files and gems.

```
require 'pry'
require 'nokogiri'
require 'open-uri' 
require 'date'
require 'colorize'

require_relative "./recent_earthquakes/version"
require_relative './recent_earthquakes/cli'
require_relative './recent_earthquakes/earthquakes'
require_relative './recent_earthquakes/scraper'
```

**CLI Class**

CLI class's 
```
.call
```
method lists the most recent eartquakes with 
```
list_recent_earthquakes 
```
method and activates the 
```
menu
```
. 
```
list_recent_earthquakes 
```
calls
```
.scrape_earthquakes
```
on
```
Scraper
```
class.

```
class RecentEarthquakes::CLI #CLI Controller

  def call
    list_recent_earthquakes
    menu
  end

  def list_recent_earthquakes
    Scraper.scrape_earthquakes
    system("clear")
    puts "\nRecent Earthquakes around the world!"
    puts "\n======================================================================== Magnitude === Local Time/Date ===\n".colorize(:green)
    @earthquakes = RecentEarthquakes::Earthquake.recent
    @earthquakes.map.with_index(1) do |earthquake, i|
      printf(" %-2s - %-70s%3s    %20s\n", i, earthquake.location, earthquake.magnitude.colorize(:red), earthquake.local_time.colorize(:yellow))
    end
    puts "\n"
  end

  def menu
    input = nil
    while input != "exit" 
    puts "==========================================================================================================\n".colorize(:green)
    puts "Enter the number(1-15) of the location you'd like more info on or type 'list' to see most recent earthquakes:"
      input = gets.strip

      if input.to_i > 0 && input.to_i < 16
        i = input.to_i - 1
        puts "\n"
        puts @earthquakes[i].location.colorize(:red)
        puts "======= Magnitude ===== Depth ===== Population ===== Elapsed Time ===== Local Time - Time Standart =======\n".colorize(:green)
        printf("%28s%29s%31s%33s%43s%18s\n", @earthquakes[i].magnitude.colorize(:red), @earthquakes[i].depth.colorize(:red), @earthquakes[i].population.colorize(:red), @earthquakes[i].elapsed_time.colorize(:red), @earthquakes[i].local_time.colorize(:yellow), @earthquakes[i].time_standart.colorize(:yellow))
        puts "\n"
      elsif input == "list"
        RecentEarthquakes::Earthquake.clear
        list_recent_earthquakes
      elsif input.downcase.strip == "exit"
        goodbye
      else
        puts "\nPlease enter a valid command, 'list' or 'exit':"
      end
    end
  end

  def goodbye
    puts "Goodbye"
  end
end
```

**Scraper Class**

The scraper scrapes a Nokogiri object from targeted website, parses the data and colloborates with earthquake class to create a earthquake class instances.

```
class Scraper

  def self.scrape(index_url)
    doc = Nokogiri::HTML(open(index_url).read, nil, 'utf-8') 
    create_new_earthquakes(doc)
  end

  def self.scrape_earthquakes
    index_url = "http://m.emsc.eu/earthquake/latest.php"
    self.scrape(index_url)
  end

  def self.parser(doc)
    (5..61).step(4).to_a.map {|x| doc.css('table tr')[x].text.gsub(/[\t\r\n]/,"  ") }
  end

  def self.create_new_earthquakes(doc)
    parser(doc).map do |e|
      a = e.split(/\s{3,}/)
      new_earthquake = RecentEarthquakes::Earthquake.new
      new_earthquake.location = (a[1][28..-1] + ", " + a[3].split("/")[0].strip)
      new_earthquake.local_time = a[3].split("/")[2].gsub("local time:", "").strip
      new_earthquake.magnitude = a[1][23..26]
      new_earthquake.elapsed_time = a[2].split(/[FD]/).first # a[2].match(/.*o/).to_s
      new_earthquake.population = a[3].split("/")[1].gsub("pop:","").strip
      new_earthquake.depth = a[2][a[2].index(":")..-1].gsub(":", "")
      new_earthquake.time_standart = a[1].split(" ")[1]
      new_earthquake.save
    end
  end
end

```

**Earthquake Class**

Earthquake class is rather simple it holds the earthquake instances with 7 different attributes and saves earthquakes inside a class variable.

```
class RecentEarthquakes::Earthquake
  attr_accessor :location, :local_time, :magnitude, :elapsed_time, :population, :depth, :time_standart
  @@earthquakes = []

  def self.recent
    @@earthquakes
  end

  def save
    @@earthquakes << self
    self
  end
end

```


[~aehmt](https://github.com/aehmt/)

