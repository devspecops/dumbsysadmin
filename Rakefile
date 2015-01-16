require 'date'
require 'html/proofer'

ROOT = File.expand_path(File.dirname(__FILE__))

desc %q{New screenshot - rake shot [desktop=true] [timer=10] [file=screenshot.png]}
task :shot do
  file    = ENV['file']    || 'screenshot.png'
  timer   = ENV['timer']   || 10
  desktop = ENV['desktop'] || 'false'
  path    = File.join(ROOT, 'images', file)
  file =~ /\.(.+)/
  format = $1
  sh "screencapture #{(desktop == 'true' && "-T #{timer}") || '-w -o'} -t #{format} #{path}"
end

desc %q{New post - rake post [title="new post"]}
task :post do
  title  = ENV['title'] || 'new post'
  file   = title.gsub(/\s+/, '-').downcase
  format = '%d-%02d-%02d-%s.md'
  date   = DateTime.now
  path   = File.join(ROOT, '_posts', sprintf(format, date.year, date.month, date.day, file))
  if File.exists? path
    raise Exception, "Can not create a new post. File #{path} exists."
  end
  File.open(path, 'w') do |file|
    file.puts <<EOF
---
excerpt: Needs an excerpt
published: false
categories: [dev, ops]
---

# #{title}
***

EOF
  end
end

desc 'Run Jekyll server'
task :server do
  sh 'bundle exec jekyll serve'
end

desc 'Build Jekyll'
task :build_jekyll do
  sh 'bundle exec jekyll build -q'
end

desc 'HTML Test'
task :test_html => :build do
  path = File.join(ROOT, '_site')
  HTML::Proofer.new(path).run
end

desc 'Test'
task :test => :test_html

desc 'Build'
task :build => :build_jekyll

namespace :ci do
  desc 'Build'
  task :build => :build_jekyll

  desc 'Test'
  task :test => :test_html
end
