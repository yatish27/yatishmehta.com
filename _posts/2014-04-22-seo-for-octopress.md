---
layout: post
title: SEO for Octopress on Heroku
summary: Additional changes are required to make Octopress more SEO friendly.
---

I recently shifted to octopress and I am loving it. It is an awesome blogging platform for developers.
Cool looking layout ,simple to use, awesome use of HTML5 and CSS3.
But there are some hidden features and changes that need to be improved for a better SEO.

## Metadata description for every blog

When a post file is generate with the command `rake new_post[title]`,the keys layout, title, date, comments and
categories  are automatically created, but if you add two more keys "keywords" and "description" it automatically
creates meta tags for that corresponding post

{% highlight yaml %}
---
layout: post
title: "SEO for Octopress"
date: 2012-04-22 09:55
comments: true
categories: [seo,octopress]
keywords: seo,octopress,heroku
description: How to optimize Octopress for SEO,Heroku
---
{% endhighlight %}
Hence the generated html of that particular post will be as follows

{% highlight html %}
<title>SEO for Octopress </title>
<meta name="author" content="Yatish Mehta">
<meta name="description" content="How to optimize Octopress for SEO">
<meta name="keywords" content="seo,octopress">
{% endhighlight %}

## Metadata for home page

The above method creates meta tags of keywords and description for every post, but it does not create for the home page,
hence a bit of hack is required to get it done.


  * Change the source/head.html
    
    {% highlight html %}
    {% raw  %}
    <meta name="author" content="{{ site.author }}">
    {% capture description %}
    {% if page.description %}
      {{ page.description }}
    {% elsif site.description %}
      {{ site.description }}
    {% else %}
      {{ content | raw_content }}
    {% endif %}
    {% endcapture %}
    
    <meta name="description" content="{{ description | strip_html | condense_spaces | truncate:150 }}">
    {% if page.keywords %}
      <meta name="keywords" content="{{ page.keywords }}">
    {% else %}
      <meta name="keywords" content="{{ site.keywords }}">
    {% endif %}
    
    {% endraw %}
    {% endhighlight %}
    
  * Add two keys in _config.yml
  
    ```
    url: http://www.yatishmehta.in
    title: Yatish Mehta
    subtitle:  Hack to live
    author: Yatish Mehta
    simple_search: http://google.com/search
    description: Tips, tricks, and hacks on Ruby on Rails development.
    keywords: ruby,ruby on rails,salesforce,gem,web development,Ajax,SEO,scraping
    ```
    This will add keywords and description in your homepage as well which very important considering the SEO
    
    
## Redirect to single domain

At times your site is available on both domains example 'http://www.yatishmehta.com' and 'http://yatishmehta.com'.
In such scenario, you are conflicting with yourself for unique content and this may hamper your rating.
You should choose one of them as your main domain and redirect the other one to it. I have hosted my site on Heroku.
The way to do it on Heroku is as follows

Include gem 'rack-rewrite' in your Gemfile

{% highlight ruby %}
gem 'rack-rewrite'
{% endhighlight %}

Make the following changes in your `config.ru` file. I have decided http://www.yatishmehta.com to be my primary domain.

{% highlight ruby %}
require 'bundler/setup'
require 'sinatra/base'
require "rack-rewrite"
# The project root directory
$root = ::File.dirname(__FILE__)
ENV['RACK_ENV'] ||= 'development'
ENV['SITE_URL'] ||= 'www.yatishmehta.in'
use Rack::Rewrite do
  r301 %r{.*}, "http://#{ENV['SITE_URL']}$&", :if => Proc.new { |rack_env|
    ENV['RACK_ENV'] == 'production' && rack_env['SERVER_NAME'] != ENV['SITE_URL']
  }
  r301 %r{^(.+)/$}, '$1'
end
{% endhighlight %}