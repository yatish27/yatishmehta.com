require "rubygems"
require "tmpdir"

require "bundler/setup"
require "jekyll"


# Change your GitHub reponame
GITHUB_REPONAME = "yatish27/yatish27.github.io"


desc "Generate blog files"
task :generate do
  Jekyll::Site.new(Jekyll.configuration({
                                            "source"      => ".",
                                            "destination" => "_site"
                                        })).process
end

desc "Generate and publish blog to gh-pages"
task :publish => [:generate] do
  Dir.mktmpdir do |tmp|
    cp_r "_site/.", tmp

    pwd = Dir.pwd
    Dir.chdir tmp

    system "git init"
    system "git add ."
    message = "Site updated at #{Time.now.utc}"
    system "git commit -m #{message.inspect}"
    system "git remote add origin git@github.com:#{GITHUB_REPONAME}.git"
    system "git push origin master --force"

    Dir.chdir pwd
  end
end

desc 'create new post or bit. args: type (post, link), title, future (# of days)'
# rake new type=(bit|post) future=0 title="New post title goes here" slug="slug-override-title"
task :new do
  type = ENV["type"] || "link"
  title = ENV["title"] || "New Title"
  future = ENV["future"] || 0
  slug = title.gsub(' ','-').downcase


  TARGET_DIR = "_posts"


  filename = "#{Time.new.strftime('%Y-%m-%d')}-#{slug}.md"
  path = File.join(TARGET_DIR, filename)
  post = <<-HTML
---
layout: TYPE
title: "TITLE"
date: DATE
---
  HTML
  post.gsub!('TITLE', title).gsub!('DATE', Time.new.to_s).gsub!('TYPE', type)
  File.open(path, 'w') do |file|
    file.puts post
  end
  puts "new #{type} generated in #{path}"
  system "open -a subl #{path}"
end