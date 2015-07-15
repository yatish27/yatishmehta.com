---
layout: post
title: Linkedin Scraper
summary: Scraping Linkedin public profiles for data validation.
---

Scraping linkedin  public profiles for data validation.

There are many use cases when the data from [Linkedin](http://www.linkedin.com) needs to be 
scraped for data collection or contact validation. A gem in ruby called 'linkedin-scraper' helps to scrape off 
the public profile when the link of a certain profile is given.
Here's how

First install the gem

{% highlight ruby %}
gem install linkedin-scraper
{% endhighlight %}

Once the gem is installed, you can access it using the following code


{% highlight ruby %}
require 'linkedin-scraper'
profile = Linkedin::Profile.new("in.linkedin.com/pub/yatish-mehta/22/460/a86")

profile.first_name          
profile.last_name          
profile.title              
profile.location           
profile.country             
profile.industry 
profile.linkedin_url           

# Array of hash containing its past job companies and job profile
# Example
#  [
#    [0] {
#          :past_title => "Intern",
#        :past_company => "Sungard"
#        },
#    [1] {
#          :past_title => "Software Developer",
#        :past_company => "XYZ"
#        }
#  ]
profile.past_companies

# Array of hash containing its current job companies and job profile
# Example
#  [
#    [0] {
#          :current_title => "Intern",
#        :current_company => "Sungard"
#        },
#    [1] {
#          :current_title => "Software Developer",
#        :current_company => "ABC"
#        }
#  ]
profile.current_companies

# It is the list of visitors "Viewers of this profile also viewed"
profile.recommended_visitors 
{% endhighlight %}


