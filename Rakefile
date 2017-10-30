# encoding: utf-8

require 'rubygems'
require 'bundler'
require 'bundler/gem_tasks'

begin
  Bundler.setup(:default)
rescue Bundler::BundlerError => e
  $stderr.puts e.message
  $stderr.puts "Run `bundle install` to install missing gems"
  exit e.status_code
end

require 'rdoc/task'
RDoc::Task.new do |rdoc|
  version = File.exist?('VERSION') ? File.read('VERSION') : ""

  rdoc.rdoc_dir = 'rdoc'
  rdoc.title = "bugsnag #{version}"
  rdoc.rdoc_files.include('README*')
  rdoc.rdoc_files.include('lib/**/*.rb')
end

# RSpec tasks
require 'rspec/core'
require "rspec/core/rake_task"
RSpec::Core::RakeTask.new(:spec) do |task|
  exclude_patterns = []
  begin
    require 'sidekiq/testing'
  rescue LoadError
    puts "Skipping sidekiq tests, missing dependencies"
    exclude_patterns << "**/integrations/sidekiq_spec.rb"
  end
  begin
    require 'delayed_job'
  rescue LoadError
    puts "Skipping delayed_job tests, missing dependencies"
    exclude_patterns << "**/integrations/delayed_job_spec.rb"
  end
  if exclude_patterns.length > 0
    task.rspec_opts = " --exclude-pattern '#{exclude_patterns.join(',')}'"
  end
end

task :default  => :spec
