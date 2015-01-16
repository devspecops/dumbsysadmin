---
excerpt: CodeStrap a Project and Text file initialisation tool
published: true
categories: [dev, ops, projects]
---

# Project Genesis - CodeStrap

[http://www.github.com/dexterp/codestrap](http://www.github.com/dexterp/codestrap) - A command line Code Generator

---

## Introduction

CodeStrap is a code generator that generates boilerplate for individual text files and project code.

### Disclaimer - Under development

CodeStrap is still under development. So if your planning on using it please note that the syntax may change to better
support the end user.

I'm hoping to release soon but I wont commit to any dates.

### Personal Experience

At the time I started my Career I had the option of either becoming a pure coder or Systems Administrator. Since
Systems Administration was the only option that provided both, I decided to go down that route.
I loved both disciplines and couldn't give up one over the other.

Since that decision I've written a lot of code, whether it be the ISP automation, writing code for an internal
automation tool in a large TelCo, authoring a internal monitoring tool that covered a large Dot Com, simple web
applications and dealing with various connectors for REST, SOAP, JSON-RPC in any one of the following scripting
languages Ruby, Python, Perl.

Given I was writing a lot of code, I wondered why there wasn't a generic tool to generate project "furnishings" outside
of "Integrated Development Environments" or editors such as Vim.

I really wanted a tool that could be used without an IDE or editor, a tool that would be simple to use on the command
line, from any location on the file system.

### Project furnishings

I define project "furnishings" as any written text outside the functional components of a project. For software projects
another common term is "Boilerplate code" however not all text that make up a project is code.

Furnishings can be embedded text, such as documentation markup or supporting files such as a
[Rakefile](http://martinfowler.com/articles/rake.html) file or the *Vagrantfile* from the
[Vagrant](https://www.vagrantup.com/) project.

For example take what might be the typical file structure to developing a simple Ruby Gem...

```
rspec/test_spec.rb
lib
tmp/
.env
.gitignore
.ruby-gemset
.ruby-version
Gemfile
stub.gemspec
Vagrantfile
Rakefile
```

Its plain to see there is a lot of files which do not constitute the functional project code.

Many delay or completely avoid adding the furnishings because of the effort required, which is a shame. Its the
furnishings that make a project manageable/maintainable and most furnishings have repeatable patterns.

### Rakefile effort example

To demonstrate the amount of work required to add project furnishings, lets count the number of lines of source.
This is a Rake task file from the popular Puppet project that creates benchmark reports, no doubt a useful task but
requiring a 139 lines of code. This is just one of the many tasks included in the Puppet project.
This is the kind of code that could be reused given a means of delivery to new projects.

```
wget https://raw.githubusercontent.com/puppetlabs/puppet/master/tasks/benchmark.rake -O - -q | wc
     139     409    4317
```

# Command line usage pattern

Many hackers that use Linux/Unix treat the command line as the only IDE they will ever need, this is somewhat my view as
well. While I've moved on to using [Rubymine](https://www.jetbrains.com/ruby/), I still use the command line heavily for
development. I haven't completely left my favourite editor either (Vim) as the only way I was going to move from the
command line is if the IDE I would move to had [Vim emulation](http://blog.jetbrains.com/ruby/2009/08/rubymine-for-vim-addicts/).

Two of the most useful features on the command line is
[command line completion](http://www.thegeekstuff.com/2013/12/bash-completion-complete/) and
[command line history](http://lifehacker.com/278888/ctrl%252Br-to-search-and-other-terminal-history-tricks). The
advantage of having these two features is a developer only has to remember parts of a command and the execution of
historical commands can be done with ease. These are features I wanted to incorporate with CodeStrap.

CodeStrap was written with double tap (&lt;Tab&gt;&lt;Tab&gt;) in mind so that developers only have to remember two
commands.

 * strap - Used to generate new project furnishings
 * stub - Used to generate new text file furnishings

In order to access the boilerplate the user simply needs to type...

> strap&lt;Tab&gt;&lt;Tab&gt;


```bash
strap*PROJECT* $NEW_DIRECTORY

# E.G.

strappuppetmodule apachemodule
straprubygem mygem
```

or...

> stub&lt;Tab&gt;&lt;Tab&gt;

```bash
stub*TEMPLATE* $NEW_FILE

# E.G.

stubrubyscript script.rb
stubpythonscript script.py
stubperlscript script.pl
```

For generating boilerplate code it doesn't get much easier then a command plus an argument.

### Does this mean a I have to create my own scripts?

No. Every strap... or stub... command is a symlink to the "strap" script.

```bash
cd {...}/bin
ln -s `which strap` strapnewproject
ln -s `which strap` stubnewscript
```

This allows for arbitrary commands can be created, which subsequently renders the corresponding file or project
template when the command is invoked.

```bash
# Code generator for a project
$HOME/.codestrap/bin/straprubygem
# Corresponding project template
$HOME/.codestrap/content/rubygem/

# Code generator for a text file
$HOME/.codestrap/bin/stubrubyscript
# Corresponding file template
$HOME/.codestrap/content/rubyscript.erb
```

The "strap" command provides a option to synchronizes the symlinks with templates in the *contents* directory.

```bash
strap --generate
```

# Setup

## Prerequisites

 * Ruby

## Install

Install developer dependencies.

```bash
gem install bundler
gem install rake
```

Download the source and install.

```bash
git clone https://github.com/dexterp/codestrap.git
bundle install
rake build
gem install pkg/codestrap-0.1.0.gem
```

Verify CodeStrap is working

```bash
strap --version
```

# Setup

Create CodeStrap default directory.

```bash
mkdir $HOME/.codestrap
```

## Content space

```bash
mkdir $HOME/.codestrap/content
```

The content space is a directory to place project and text boilerplate.

Creating Project boilerplate simple as it is just a directory. For example to create a boilerplate template named
"rubygem" simply make a directory and place files and directories under it.

```bash
cd $HOME/.codestrap/content
bundle gem rubygem
     create  rubygem/Gemfile
     create  rubygem/Rakefile
     create  rubygem/LICENSE.txt
     create  rubygem/README.md
     create  rubygem/.gitignore
     create  rubygem/rubygem.gemspec
     create  rubygem/lib/rubygem.rb
     create  rubygem/lib/rubygem/version.rb
```

To create text file boilerplate is just as simple as it is an ERB file in the content directory. For example creating
a boilerplate named "rubyscript" simply create a file named "rubyscript.erb" in the content directory.

```bash
cd $HOME/.codestrap/content
echo <<EOF > rubyscript.erb
#!/usr/bin/env ruby

puts "Hello World\n"
EOF
```

You are probably asking if there is a code generator associated with the tool you are building with why not use that
instead of CodeStrap?

The answer is that most built in code generators have no way to create custom content. For example including a
Vagrantfile or even a custom Rakefile to facilitate local development isn't available through bundler. Consider the
potential permutations various developers may include into the boilerplate mix and it becomes obvious its difficult for
default code generators to include all these components.

CodeStrap strength comes from its flexibility to add custom content.

## Objects

Template objects (objects that are used in ERB files) are either...

* File derived
* Builtin

### Objects - File derived

Objects can be created from static text files residing in the "objects" directory.

```bash
mkdir $HOME/.codestrap/objects
```

The files in this directory is one of three types...

* YAML
* JSON
* Executable script with JSON output

The name of the file is significant. What ever the file is named minus the postfix (eg .yaml, .json, .rb etc) forms the
object name within the template.

#### Objects - Derived from YAML

For a file named ...

```
contact.yaml
```

with content of ...

```yaml
---
name: Full Name
email: full.name@the.cloud
```

forms an object in Ruby similar to ...

```ruby
require 'yaml'
require 'ostruct'

text = File.read('contact.yaml')
obj = YAML.load(text)

contact = nil
if obj.is_a?(Hash)
  contact = OpenStruct.new(obj)
else
  contact = obj
end

if contact.is_a?(OpenStruct)
  puts contact.name
  puts contact.email
end
```

#### Objects - Derived from JSON

For a file named ...

```
work.json
```

with content of ...

```json
{
  "name": "Full Name",
  "email": "full.name@the.cloud"
}
```

forms an object in Ruby...

```ruby
require 'json'
require 'ostruct'

text = File.read('work.json')
obj = JSON.load(text)

work = nil
if obj.is_a?(Hash)
  work = OpenStruct.new(obj)
else
  work = obj
end

if work.is_a?(OpenStruct)
  puts work.name
  puts work.email
end
```

#### Objects - Derived from executables

Any script that generates JSON to standard output can be utilized to create objects. Script objects provides channel
those who do not code in Ruby to create objects dynamically without getting into the Ruby API.

Given an executable file named...

```
developer.py
```

with the execute bit turned on and a content of ...

```python
#!/usr/bin/env python

import json
print(json.dumps({ 'name': 'Full Name', 'email': 'full.name@developer.address' }))
```

when executed through a system call, forms an object in Ruby similar to ...

```ruby
require 'json'
require 'ostruct'

text = %x(#{developer.py})
obj = JSON.load(text)

developer = nil
if obj.is_a?(Hash)
  developer = OpenStruct.new(obj)
else
  developer = obj
end

if developer.is_a?(OpenStruct)
  puts developer.name
  puts developer.email
end
```

#### Template Namespace

A deliberate decision was made to use "Objects" instead of "String" types in the template namespace because the Template
namespace can become quickly polluted.

Note for Ruby coders: Many will probably be quick to point out that Strings also form objects, the more precise way to
describe this is that no String objects are created in the root namespace.

### Objects - Builtin

As not all objects can be created from YAML, JSON or script files, there are built in objects in CodeStrap. These
objects are dynamic in nature or cant be derived from a simple file.

#### Project Object

This object provides metadata for the strap... or stub... new project or file.

```ruby
# Module
# The argument passed to stub... or strap...
# E.G. straprubygem newgem has a project name of newgem
project.module

# Name
# The argument passed to stub... or strap... but with the directory name and postfix stripped off.
# E.G. stubrubyscript /tmp/foo.rb will result in a project.name of "foo"
project.name
```

#### Date Object

This is the DateTime object from ruby

```ruby
require 'date'

datetime = DateTime.new
```

## Binary directory

CodeStrap needs bin directory where commands (symlinks) are stored. The default location is...

```bash
mkdir $HOME/.codestrap/bin
```

Make sure to modify $PATH to include the directory.

```bash
export PATH=$HOME/.codestrap/bin:$PATH
```

# Example - Individual file boilerplate

First a template has to be created

```bash
touch $HOME/.codestrap/content/rubyscript.erb
```

Populate with ruby ERB content.

```erb
#!/usr/bin/env ruby

require 'optparse'

options = {}
OptionParser.new do |opts|
  opts.banner = 'Usage <%= project.name %> [options]'

  opts.on('-?', '--help', 'Print Help and exit') do
    puts opts
    exit 0
  end

  opts.on('--author', 'Print authors name exit') do
    puts '<%= author.name %>'
    exit 0
  end

  opts.on('--email', 'Print authors email and exit') do
    puts '<%= author.email %>'
    exit 0
  end

end.parse!
```

Since we have determined that the template should contain the author object, the author object has to be defined. This
done by populating a static file in the *objects* directory.

```bash
cat <<EOF > $HOME/.codestrap/objects/author.yaml
name: The masked avenger
email: bat.man@the.cloud
EOF
```

Manually or automatically generate the symlink.

```bash
# Manually
ln -s `which stub` $HOME/.codestrap/bin/stubrubyscript
# Automatically
strap --generate
```

Make sure that the *bin* directory has been put into $PATH

```bash
export PATH=$HOME/.codestrap/bin:$PATH
```

Start generating some scripts

```bash
stubrubyscript /usr/local/bin/mynewscript.rb
stubrubyscript /usr/local/bin/testscript.rb

# Switch execute bit on
chmod 755 /usr/local/bin/{mynewscript,testscript}.rb
```

Test your new scripts by seeing the option outputs.

```bash
$ mynewscript.rb --help
Usage mynewscript.rb [options]
   -?, --help                       Print Help and exit
       --author                     Print authors name exit
       --email                      Print authors email and exit

$ mynewscript.rb --author
The masked avenger

$ mynewscript.rb --email
bat.man@the.cloud
```

# Example - Project boilerplate

Use puppets default generate command to generate a module

```bash
cd $HOME/.codestrap/content
puppet module generate author/puppetmodule
mv author-puppetmodule puppetmodule
```

Edit the puppetmodule/metadata.json file components with template data.

```erb
# stub:erb
{
  "name": "<%= puppet.username %>/<%= project.name %>",
  "version": "0.1.0",
  "author": "<%= puppet.name %>",
  "summary": "Template module",
  "license": "Apache 2.0",
  "source": "",
  "project_page": null,
  "issues_url": null,
  "dependencies": [
    {"name":"puppetlabs-stdlib","version_requirement":">= 1.0.0"}
  ]
}
```

Note the *stub:erb* line. This is called a modeline, its purpose is to tell CodeStrap that the file has to be render.
This was the simple option over files with a .erb extension as trying to escape ERB files needed within the project
would not have been easy.

Edit the puppetmodule/manifests/init.pp file.

```puppet
# == Class: <%= project.name %>
#
# Full description of class test here.
#
# === Parameters
#
# Document parameters here.
#
# [*sample_parameter*]
#   Explanation of what this parameter affects and what it defaults to.
#   e.g. "Specify one or more upstream ntp servers as an array."
#
# === Variables
#
# Here you should define a list of variables that this module would require.
#
# [*sample_variable*]
#   Explanation of how this variable affects the funtion of this class and if
#   it has a default. e.g. "The parameter enc_ntp_servers must be set by the
#   External Node Classifier as a comma separated list of hostnames." (Note,
#   global variables should be avoided in favor of class parameters as
#   of Puppet 2.6.)
#
# === Examples
#
#  class { '<%= project.name %>':
#    servers => [ 'pool.ntp.org', 'ntp.local.company.com' ],
#  }
#
# === Authors
#
# <%= puppet.name %> <<%= puppet.email %>>
#
# === Copyright
#
# Copyright 2015 Your name here, unless otherwise noted.
#
class <%= project.name %> {
  anchor { '<%= project.name %>::start': } ->
  class { '<%= project.name %>::config': } ->
  anchor { '<%= project.name %>::end': }
}
```

We are also going to add a directory that has a special syntax to identify objects that can be interpolated.

```bash
cd $HOME/.codestrap/content/puppetmodule/manifests
mkdir :strap:project.name:
```

I know this is a pretty weird looking directory but I couldn't think of an idiom that would make it easy to interpolate
directories. Open to suggestions if anyone has a good one.

Create a config.pp file in the freshly created directory

```bash
cd :strap:project.name:
echo <<EOF > config.pp
class <%= project.name %>::config {

}
EOF
```

The last file to add is a Vagrantfile. This is the kind of file that a built in code generator distributed with a core
framework would not create.

```bash
cat <<EOF > $HOME/.codestrap/content/puppetmodule/Vagrantfile
# vim: filetype=ruby

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "parallels/ubuntu-14.04"
end
EOF
```

Now we need to create some contact details by populating the file

```
$HOME/.codestrap/objects/puppet.yaml
```

```yaml
username: author
name: Full Name
email: full.name@the.cloud
```

Manually or automatically generate the symlink.

```bash
# Manually
ln -s `which stub` $HOME/.codestrap/bin/strappuppetmodule
# Automatically
strap --generate
```

Generate some puppet modules

```bash
mkdir $HOME/workspace
strappuppetmodule $HOME/workspace/apache
strappuppetmodule $HOME/workspace/mysql
```

Now examine the contents of your modules and look for the interpolated content.

# Conclusion

I hope you liked this journey feel free to add me on linked in and drop me a line.

CodeStrap has a client/server feature built into the code. I'll reserve that for the next blog post.
As stated before CodeStrap is still under development. There is still a bit of work to do, but hopefully you get the
idea.
