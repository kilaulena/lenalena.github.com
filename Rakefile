desc 'Generate tags page'
task :tags do
  puts "Generating tags..."
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters

  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read_posts('')
  site.categories.sort.each do |category, posts|

    next if category == ".net"
    html = ''
    html << <<-HTML
---
layout: default
title: Entries tagged "#{category}"
type: "#{category.gsub(/\b\w/){$&.upcase}}"
---
    <h2 class="posts-by-tag">Entries tagged "#{category}"</h2>
    HTML
  
    
    posts.reverse.each do |post|
      post_data = post.to_liquid
      html << <<-HTML
        <div class="atomentry">
          <div class="titlebar">
            <h2 class="title">
              <a href="#{@@site_url}/#{post.url}" rel="tag" title="#{post_data['title']}">#{post_data['title']}</a>
            </h2>
          </div>

          <div class="author">
            Posted by <cite>Lena Herrmann</cite> <abbr>on #{post_data['date'].strftime("%d %B %Y")}</span></abbr>
          </div>
  
          <div class="content">
            #{post_data['excerpt']}
          </div>
        </div>
      HTML
    end

    FileUtils.mkdir_p "tag/#{category}"
    File.open("tag/#{category}/index.html", 'w+') do |file|
      file.puts html
    end
  end
  puts 'Done.'
end

@@site_url = 'http://192.168.1.103'
# @@site_url = 'http://lenaherrmann.net'

desc 'Generate an include snippet with a tagcloud'
task :cloud_basic do
  puts 'Generating tag cloud...'
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters

  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read_posts('')

  html = ''

  site.categories.sort.each do |category, posts|
    s = posts.count
    font_size = 10 + (s*1.8);
    html << "<a href=\"#{@@site_url}/tag/#{category}/\" title=\"Articles tagged #{category}\" style=\"font-size: #{font_size}px; line-height:#{font_size + (font_size*0.2)}px\" rel=\"tag\">#{category}</a> "
  end

  File.open('_includes/tags.html', 'w+') do |file|
    file.puts html
  end

  puts 'Done.'
end

desc 'run jekyll'
task :jekyll do
  system "jekyll --pygments --paginate 8"
end

desc 'build tags, tag cloud, and run jekyll'
  task :prepare => [:tags, :cloud_basic, :jekyll]
