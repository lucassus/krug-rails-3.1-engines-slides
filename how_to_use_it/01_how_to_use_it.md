!SLIDE title subsection
# Jak użyć naszego engine? #

!SLIDE small
# Dodanie zależności #

## Gemfile

    @@@ ruby
    # ...
    gem 'krug_forum',
      :git => 'git@github.com:lucassus/krug_forum.git'
    # ...

## config/application.rb

    @@@ ruby
    # ...
    require 'krug_forum'
    # ...

!SLIDE small
# Podpięcie engine w routes #

## config/routes.rb

    @@@ ruby
    KrugForumDemo::Application.routes.draw do
      root :to => "index#index"

      # nasz engine będzie dostępny pod adresem '/forum'
      mount KrugForum::Engine => "/forum"

      # ...
    end

!SLIDE bullets incremental
# Migracje #

* Skopiowanie migracji do katalogu aplikacji
* `rake my_forum:install:migrations`
* ..i oczywiście ich uruchomienie
* `rake db:migrate`

!SLIDE title
# Jak używać helperów do routes #

!SLIDE
# Hepery do routes #

## W aplikacji używającej engine

    @@@ html
    <%= link_to 'Forum', krug_forum.root_path %>

## W engine

    @@@ html
    <%= link_to 'Home', main_app.root_path %>

