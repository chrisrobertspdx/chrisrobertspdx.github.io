---
layout: post
title:      "RaceFinder: a cli gem project"
date:       2017-11-29 15:25:08 -0500
permalink:  cli_gem_project
---


My gem is called Race Finder. As the name suggests it scrapes the [racecenter web site](http://www.racecenter.com) for a list of current races, presents a list of these races and gives the user the opportunity to get more information about a particular race. Version 2.0 will give the user the chance to generate the initial race list based on type and location. This is what the interface looks like:

```
1. Seward Solstice Trail Run
2. The 12K's of Christmas
3. Holiday Fun Run: 5k, 10k, 15k kids dash
4. Freeze Your Fanny

Which race would you like to know more about? 
4

RUNNING
Freeze Your Fanny | 12.30.17 | Madras, OR
---------------
Freeze Your Fanny Family fun run benefiting MountainStar Family Relief Nursery of Madras. 3mile run/walk (kids+dogs welcome) 8 mile "Prison Breakout Run" Biathlon 500y swim + 3 mile run (can compete as a team) Registration: $25 single advance ($30 day of) or $50 team advance ($60 day of) *includes running hat/chili/swim! Children 12 and Under Free! Registration 9:00am / Long run 9:30am / Short run + Biathlon 10am register now at www.mtstar.org/fyf Your participation helps to support MountainStar Family Relief Nursery in Madras!
---------------
Which race would you like to know more about? 
exit

Goodbye
```

To get the project started I typed:

```
bundle gem race_center
```

This command created the directory structure and base files for the project. One thing to keep in mind is that bundle initializes a git repo for the gem. I made the mistake of initializing a git repo before running bundle gem and it caused numerous problems and headaches.

I ended up the 4 classes: calendar, race, scraper and CLI. The calendar and race classes instantiate typical database type objects that hold the information like date, location, type etc. The CLI creates the menu that the user interacts with and also is in charge of employing the scraper methods to initialize the calendar and get more race information when prompted. 

The initial scrape builds an array of hashes - each hash corresponds to a race. The CLI is able to take this array and build all of the race instances that belong to the calendar. Here is one of my scraper functions. As is typical with scraping there is usually some sort of edge case that will break the pattern. In this case racecenter slapped an advertisement in the middle of the calendar table. Luckily it had an ID of "gad".

```
  def self.scrape_index(index)
    races = []
    html_block = self.get_page(index).css("table#calendarlist tr")
    html_block.each {|r|
      #binding.pry
      d = r.css("td")
        if d.size > 0 && d[0]["class"] != "gad" #google ad
            #binding.pry
            races << {
                :type => d[0].text,
                :date => d[1].text,
                :name => d[2].text,
                :url => d[2].css("a").attribute("href").value,
                :location => d[3].text
            }
        end
    }
    races
  end
```

I did use the generic metaprogramming approach for initializing race instances since this will make it easier to add / remove attributes in the future.

```
class RaceFinder::Race
    attr_accessor :name, :type, :date, :location, :description, :venue, :url
    def initialize(attr)
        attr.each {|key, value| self.send(("#{key}="), value)}
    end
    def update(attr)
        attr.each {|key, value| self.send(("#{key}="), value)}
    end
    def show_details
       puts "#{self.type.upcase}"
       puts "#{self.name} | #{self.date} | #{self.location}"
       puts "---------------"
       puts self.description
       puts "---------------"
    end
end
```

There was one little wrinkle with the description field. I decided that I would only scrape for the information on the detail page if the attribute was undefined. The CLI was in charge of determining whether to display current information or scrape.

```
  def get_details(index)
    this_race = self.calendar.races[index-1]
    if this_race.description == nil
      this_race.update(RaceFinder::Scraper.scrape_detail("#{BASE_URL}#{this_race.url}"))
    end
    this_race.show_details
  end
```

It was not that difficult to get this thing working in the bin console. The challenge came when it was time to build the gem.
I followed the tutorial which instructed me to fill out my gemspec and make sure I add the required depencies as either development or runtime. Building the gem was as simple as **gem build race_finder**. But uh-oh when I try to run the gem I get command not found. After a lot of face planting on my keyboard I found the two key lines on the gemspec.

```
  spec.bindir        = "exe"
  spec.executables   = spec.files.grep(%r{^exe/}) { |f| File.basename(f) }
```

For some reason bundle creates a bin directory but looks for the gem executable in an exe subdirectory. Once I figured this out you can change **exe** to **bin**. If you have created a file in the bin that shares the name of your gem you should be good to go. One other thing to keep in mind is that your git repo must be up to date when you build the gem. Otherwise it may miss an important file when it updates your path which lets you simply call **gem name** from bash after you install the gem.

```
git ls-files -z`.split("\x0").reject
```

That is pretty much it. I could never get rubygems to accept my gem so it exists only as local.
