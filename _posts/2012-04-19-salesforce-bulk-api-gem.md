---
layout: post
title: Salesforce bulk API gem
summary: Salesforce has REST API, SOAP API and the bulk API. This is Ruby client for the Salesforce bulk API.
---

Using the Salesforce bulk API for pushing many numbers of records in a single API call.

The official Heroku 'gem databasedotcom' which is used as an adapter to talk with the Salesforce's database doesn't support 
bulk API. I did a lot of research, but then I found that 'gem rforce' gem uses bulk API, which was not maintained. 
So finally, I decided to write a gem with help of existing resources where I can use ActiveRecord type methods to access 
the database along with OAuth2 protocol for authentication. 
The batch and the job governor limits are also taken care in this gem.

Using the gem is simple and straightforward.

{% highlight ruby %}
gem install salesforce_bulk_api
{% endhighlight %}
or add `gem salesforce_bulk_api` in your Gemfile.

You can use any method of authentication by using this gem

* Username and password combination
* OmniAuth
* Oauth2

Please go through the entire documentation of [databasedotcom](https://github.com/heroku/databasedotcom) on the various ways of authentication

{% highlight ruby %}
require 'salesforce_bulk_api'

client = Databasedotcom::Client.new :client_id =>  $SFDC_APP_CONFIG["client_id"], :client_secret => $SFDC_APP_CONFIG["client_secret"] #client_id and client_secret respectively
client.authenticate :token => "my-oauth-token", :instance_url => "http://na1.salesforce.com"  #=> "my-oauth-token"
salesforce = SalesforceBulkApi::Api.new(client)
{% endhighlight %}

Once `salesforce` object is authenticated, one can use it for various interactions with the Salesforce database.

{% highlight ruby %}
# Insert/Create
new_account = Hash["name" => "Test Account", "type" => "Other"] # Add as many fields per record as needed.
records_to_insert = Array.new
records_to_insert.push(new_account) # You can add as many records as you want here, just keep in mind that Salesforce has governor limits.
result = salesforce.create("Account", records_to_insert)
puts "result is: #{result.inspect}"

# Update
updated_account = Hash["name" => "Test Account -- Updated", id => "a00A0001009zA2m"] # Nearly identical to an insert, but we need to pass the salesforce id.
records_to_update = Array.new
records_to_update.push(updated_account)
salesforce.update("Account", records_to_update)

# Upsert
upserted_account = Hash["name" => "Test Account -- Upserted", "External_Field_Name" => "123456"] # Fields to be updated. External field must be included
records_to_upsert = Array.new
records_to_upsert.push(upserted_account)
salesforce.upsert("Account", records_to_upsert, "External_Field_Name") # Note that upsert accepts an extra parameter for the external field name

# Delete
deleted_account = Hash["id" => "a00A0001009zA2m"] # We only specify the id of the records to delete
records_to_delete = Array.new
records_to_delete.push(deleted_account)
salesforce.delete("Account", records_to_delete)

# Query
res = salesforce.query("Account", "select id, name, createddate from Account limit 3") # We just need to pass the sobject name and the query string
{% endhighlight %}
You can find the gem at [github](https://github.com/yatishmehta27/salesforce_bulk_api)
You can use the databasedotcom gem for simple purpose i.e. single API calls.