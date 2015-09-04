---
layout: post
title: Linkedin Scraper
summary: Scraping Linkedin public profiles for data validation.
---

For current porject, I needed to scrape information from Linkedin's public profile. 
Other than my project, LinkedIn data is used in many scenarios like contact validation, recruitment. 
I decided to package this utility into a rubygem. Linkedin-Scraper gem collects public information into a ruby object. 
It has ActiveRecord-like methods to access it. It also has a binary file to return information in JSON format.

## Usage
You can install the gem using

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

Many more methods are available. Check the [this](https://github.com/yatish27/linkedin-scraper) link for the source.


