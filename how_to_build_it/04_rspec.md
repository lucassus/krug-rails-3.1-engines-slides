!SLIDE title
# Instalowanie RSpec #

!SLIDE small
# RSpec: dodanie zależności #

## krug_forum.gemspec

    @@@ ruby
    Gem::Specification.new do |s|
      s.add_development_dependency "rspec"
      s.add_development_dependency "rspec-rails"
    end

## Gemfile

    @@@ ruby
    source "http://rubygems.org"

    gem 'rails', '3.1.0.rc4'
    gem 'sqlite3'
    gem 'rake', '0.8.7'

    gemspec

!SLIDE
# RSpec: instalacja #

    @@@ sh
    rails g rspec:install

## Przenieść test/dummy do spec/dumm
## Usunąć katalog test

!SLIDE bullets
# Ustawienie ścieżek do spec/dummy #

* script/rails
* Rakefile
* spec/spec_helper.rb

!SLIDE smaller
# Generatory w lib/krug_forum/engine.rb #

    @@@ ruby
    module KrugForum
      class Engine < Rails::Engine
        isolate_namespace KrugForum

        config.generators do |g|
          g.javascripts false
          g.stylesheets false
          g.helper false
          g.integration_tool false
          g.test_framework :rspec, :fixture => false
          g.fixture_replacement :factory_girl, 
            :dir => "spec/support/factories"
        end
      end
    end

!SLIDE smaller
# Scaffold dla Topic #

    @@@ sh
    rails g scaffold post subject:string body:test
          invoke  active_record
          create    db/migrate/20110627133531_create_krug_forum_posts.rb
          create    app/models/krug_forum/post.rb
          invoke    rspec
          create      spec/models/krug_forum/post_spec.rb
          invoke      factory_girl
          create        spec/support/factories/krug_forum_posts.rb
          route  resources :posts
          invoke  scaffold_controller
          create    app/controllers/krug_forum/posts_controller.rb
          invoke    erb
          create      app/views/krug_forum/posts
          create      app/views/krug_forum/posts/index.html.erb
          create      app/views/krug_forum/posts/edit.html.erb
          create      app/views/krug_forum/posts/show.html.erb
          create      app/views/krug_forum/posts/new.html.erb
          create      app/views/krug_forum/posts/_form.html.erb
          invoke    rspec
          create      spec/controllers/krug_forum/posts_controller_spec.rb
          create      spec/views/krug_forum/posts/edit.html.erb_spec.rb
          create      spec/views/krug_forum/posts/index.html.erb_spec.rb
          create      spec/views/krug_forum/posts/new.html.erb_spec.rb
          create      spec/views/krug_forum/posts/show.html.erb_spec.rb
          create      spec/routing/krug_forum/posts_routing_spec.rb
          invoke  assets
          invoke    css
          invoke  css

!SLIDE bullets incremental
# Poprawki po scaffold #

* specs nie zostały wygenerowane w odpowiednich modułach
* klasy modeli nie są w odpowiednich przestrzeniach nazw
* tj. zamienić np. `Post` na `KrugForum::Post`
* nie działają testy do kontrolerów i routes

!SLIDE small
# Testowanie routes #

## spec/spec_helper.rb

    @@@ ruby
    RSpec.configure do |config|
      config.include(KrugForum::Engine.routes.mounted_helpers)
      config.include(KrugForum::Engine.routes.url_helpers)
    end

## spec/dummy/config/routes.rb

    @@@ ruby
    # include named routes in the test env
    # see: https://github.com/rails/rails/issues/1265
    if Rails.env.test?
      namespace :krug_forum do
        resources :posts
      end
    end

!SLIDE bullets incremental
# Poprawki cd. #

* ..a później się jeszcze okaże, że nie działają:
* `rake db:migrate`
* `rake db:test:prepare`

!SLIDE
# Wskrzeszenie rake #

## Gemfile

    @@@ ruby
    gem 'rake', '0.8.7'

## Użycie

    @@@ sh
    RAILS_ENV=test rake db:migrate

!SLIDE title
# Dodawanie zależności #
## (na przykładzie inherited_resources) ##
## https://github.com/josevalim/inherited_resources ##

!SLIDE
# krug_forum.gemspec #

    @@@ ruby
    s.add_dependency "inherited_resources"

!SLIDE
# app/controllers/krug\_forum/posts\_controller.rb #

    @@@ ruby
    module KrugForum
      class PostsController < ApplicationController
        inherit_resources
      end
    end

!SLIDE bullets incremental
# lib/krug_forum/engine.rb #

    @@@ruby
    # musi się tutaj znaleźć każda zeleźność,
    # która nie jest w grupie development
    require 'inherited_resources'

    module KrugForum
      class Engine < Rails::Engine
        isolate_namespace KrugForum
      end
    end

!SLIDE title
# Jak dodać użytkowników? #
##  (na przykładzie devise) ##
## https://github.com/plataformatec/devise ##

!SLIDE bullets incremental
# Dodawanie użytkowników #

* do __Gemfile__ dodać `gem 'devise'`
* w katalogu __spec/dummy__ wygenerować devise:
* `rails generate devise:install`
* `rails generate devise User`

!SLIDE
# Rozszerzenie modelu #

    @@@ ruby
    # oczywiście najpierw musi zostać
    # stworzona migracja
    module KrugForum
      class Post < ActiveRecord::Base
        belongs_to :user
      end
    end

!SLIDE
# Rozszerzenie kontrolera #

    @@@ ruby
    module KrugForum
      class PostsController < ApplicationController
        inherit_resources

        def create
          @post = Post.new(params[:post])
          @post.user = current_user

          create! { krug_forum.posts_path }
        end
      end
    end

!SLIDE title
# ..ale co z #
# User#has_many :posts #
# ??? #

!SLIDE small
# Mixin dla modelu User #

## lib/krug\_forum/extensions/user.rb

    @@@ ruby
    module KrugForum::Extensions
      module User

        def self.included(base)
          base.send(:include, InstanceMethods)

          base.has_many :krug_forum_posts, 
            :class_name => 'KrugForum::Post'
        end

        module InstanceMethods
          def forum_posts_count
            self.krug_forum_posts.count
          end
        end

      end
    end

!SLIDE
# Mixin dla modelu User cd. #

    @@@ ruby
    class User < ActiveRecord::Base
      include KrugForum::Extensions::User
    end

!SLIDE title
# Serwowanie plików js i css #
## (na przykładzie jquery-rails) ##

!SLIDE bullets incremental
# Instalacja zależności #

* dodać `s.add_dependency 'jquery-rails'` do gemspec
* ..i nie zapomnieć o `require 'jquery-rails'` w lib/krug_forum/engine.rb

!SLIDE
# Podpięcie assets w aplikacji #

## app/assets/javascripts/krug_forum.js

    @@@ javascript
    //= require jquery
    //= require jquery_ujs

## spec/dummy/assets/javascript/application.js

    @@@ javascript
    //= require krug_forum
