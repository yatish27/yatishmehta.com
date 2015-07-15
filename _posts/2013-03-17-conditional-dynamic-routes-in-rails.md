---
layout: post
title: Conditional dynamic routes using constraints
summary: Rails routes is used to map the 'url' with proper controller and action. Its like the .htaccess for the php
         world. But many a times we require to map the same pattern of the url to different actions of different controller.
---

Rails routes is used to map the 'url' with proper controller action.
It is similar to `.htaccess` in the PHP world. There are scenarios when the same url-pattern needs to be mapped to different actions.


Consider the following the example
{% highlight ruby %}
`match "/:search" => "companies#show"`
{% endhighlight %}

This route maps to the `show` action of companies_controller. 
If we have a scenario where depending on the pattern of :search the controller action needs to be decided.
If it matches a particular Regular expression rule, it maps to action1 else action2.

{% highlight ruby %}
if :search.match/PATTERN/
  match /:search => 'companies#show'
else
  match /:search => 'technologies#show'
end
{% endhighlight %}

We can map it to single controller action and redirect it depending on the pattern from the controller.

This can also be achieved using the constraints introduced in Rails 3

{% highlight ruby %}
match "/:search" => 'companies#show', constraints: { search: /.*\..*/ }
match "/:search" => 'technologies#show'
{% endhighlight %}

Let us assume there is complex logic which decides the constraints of the routes, then the following method can be used.

Create a module or class with the `self.matches?` method. Use this method in the constraint's hash.

{% highlight ruby %}
class PatternConstraint
  def self.matches?(request)
    #
    # Any complex logic
    #
    request.path_parameters['search'].match(/REGEXP_PATTERN/)
  end
end
{% endhighlight %}

and in the routes.rb 

{% highlight ruby %}
match "/:search" => 'companies#show', constraints: PatternConstraint.new
match "/:search" => 'technologies#show'
{% endhighlight %}

One more reason to shout "Ruby on Rails is awesome"
