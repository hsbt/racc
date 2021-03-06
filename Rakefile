# -*- ruby -*-

require 'rubygems'
require 'hoe'

gem 'rake-compiler', '>= 0.4.1'
require "rake/extensiontask"

Hoe.plugin :debugging, :doofus, :git, :isolate

hoe = Hoe.spec 'racc' do
  developer 'Aaron Patterson', 'aaron@tenderlovemaking.com'

  self.extra_rdoc_files  = Dir['*.rdoc']
  self.history_file      = 'ChangeLog'
  self.readme_file       = 'README.rdoc'
  self.testlib           = :testunit

  extra_dev_deps << ['rake-compiler', '>= 0.4.1']
  extra_dev_deps << ['minitest', '~> 4.7']

  if RUBY_PLATFORM =~ /java/
    self.spec_extras = { :platform => 'java' }
  else
    self.spec_extras = {
      :extensions            => ["ext/racc/extconf.rb"]
    }
  end

  Rake::ExtensionTask.new "cparse", spec do |ext|
    ext.lib_dir = File.join 'lib', 'racc'
    ext.ext_dir = File.join 'ext', 'racc'
  end
end

hoe.test_prelude = 'gem "minitest", "~> 4.7"'

file 'lib/racc/parser-text.rb' => ['lib/racc/parser.rb'] do |t|
  source = 'lib/racc/parser.rb'

  open(t.name, 'wb') { |io|
    io.write(<<-eorb)
module Racc
  PARSER_TEXT = <<'__end_of_file__'
#{File.read(source)}
__end_of_file__
end
    eorb
  }
end

Hoe.add_include_dirs('.:lib/racc')

task :compile => 'lib/racc/parser-text.rb'
