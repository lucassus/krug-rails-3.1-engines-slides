!SLIDE title subsection
# Konfigurowanie generatorów engine #

!SLIDE smaller
# Generatory w lib/krug_forum/engine.rb #

    @@@ ruby
    module KrugForum
      class Engine < Rails::Engine
        isolate_namespace KrugForum

        config.generators do |g|
          g.javascripts false # nie generuje plików js
          g.stylesheets false # nie generuje plików css
          g.helper false # nie generuje helperów
          g.integration_tool false # nie generuje testów integracyjnych

          # RSpec + factory_girl
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

!SLIDE small
# Wygenerowana migracja #

    @@@ ruby
    class CreateKrugForumPosts < ActiveRecord::Migration
      def self.up
        create_table :krug_forum_posts do |t|
          t.string :subject
          t.text :body

          t.timestamps
        end
      end

      def self.down
        remove_table :krug_forum_posts
      end
    end

!SLIDE bullets incremental
# Poprawki po scaffold #

* Specs nie zostały wygenerowane w odpowiednich modułach
* Klasy modeli nie są w odpowiednich przestrzeniach nazw
* ...tj. zamienić np. `Post` na `KrugForum::Post`
* Nie działają testy do kontrolerów i routes

!SLIDE small
# Testowanie routes #

## spec/spec_helper.rb

    @@@ ruby
    # ...
    RSpec.configure do |config|
      config.include(KrugForum::Engine.routes.mounted_helpers)
      config.include(KrugForum::Engine.routes.url_helpers)

      # ...
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

## Recepta na `rake db:test:prepare`

    @@@ sh
    RAILS_ENV=test rake db:migrate

