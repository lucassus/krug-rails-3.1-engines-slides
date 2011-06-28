!SLIDE title subsection
# Ustawienie ścieżek do spec/dummy #

!SLIDE smaller
# script/rails #

    @@@ ruby
    #!/usr/bin/env ruby
    ENGINE_PATH = File.expand_path('../..',  __FILE__)
    load File.expand_path('../../spec/dummy/script/rails',  __FILE__)

!SLIDE smaller
# Rakefile #

    @@@ ruby
    #!/usr/bin/env rake
    require 'bundler/setup'

    APP_RAKEFILE = File.expand_path("../spec/dummy/Rakefile", __FILE__)
    load 'rails/tasks/engine.rake'

    require 'rspec/core/rake_task'
    RSpec::Core::RakeTask.new(:spec)
    task :default => :spec

!SLIDE smaller
# spec/spec_helper.rb #

    @@@ ruby
    ENV["RAILS_ENV"] ||= 'test'
    require File.expand_path("../dummy/config/environment", __FILE__)
    require 'rspec/rails'

    # ...

    RSpec.configure do |config|
      # ...
    end

