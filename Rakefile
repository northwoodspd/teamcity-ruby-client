require 'bundler/gem_tasks'
require 'rspec/core/rake_task'
require 'yard'
require 'yard/rake/yardoc_task'

RSpec::Core::RakeTask.new(:spec)

task :default => :spec

module Bundler
  class GemHelper

    # do not release to rubygems.org
    def rubygem_push(path)
      sh("gem push ./pkg/#{full_name} --host https://northwoodspd.jfrog.io/artifactory/api/gems/gems")
    end

    # skip the tag
    def tag_version
      return
    end
  end
end

def gem_helper
  @gem_helper ||= Bundler::GemHelper.new
end

def full_name
  "#{name}-#{version}.gem"
end

def name
  gem_helper.send(:name)
end

def version
  gem_helper.send(:version)
end

desc "Build #{full_name} into the pkg directory"
task :build => :spec do
  gem_helper.build_gem
end

desc "Build and install #{full_name} into system gems"
task :install => :build do
  gem_helper.install_gem
end

def sh(command)
  result = `#{command}`
  raise "#{command} failed!\n\n#{result}" unless $?.success?
end

YARD::Rake::YardocTask.new do |yardoc|
  yardoc.name = 'yard'
  yardoc.options = ['--verbose']
  yardoc.files = [
    'lib/**/*.rb'
  ]
end
