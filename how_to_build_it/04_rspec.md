!SLIDE title subsection
# Instalowanie RSpec #

!SLIDE small
# RSpec: dodanie zależności #

## krug_forum.gemspec

    @@@ ruby
    Gem::Specification.new do |s|
      # ...

      s.add_development_dependency "rspec"
      s.add_development_dependency "rspec-rails"
    end

## Gemfile

    @@@ ruby
    source "http://rubygems.org"

    gem 'rails', '3.1.0.rc4'
    gem 'sqlite3'
    gem 'rake', '0.8.7' # wft? ale o tym później..

    gemspec

!SLIDE
# RSpec: instalacja #

    @@@ sh
    rails g rspec:install

## Przenieść __test/dummy__ do __spec/dummy__
## Usunąć katalog __test__

