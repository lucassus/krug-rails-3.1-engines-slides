!SLIDE title
# Jak użyć naszego engine? #

!SLIDE small
# Dodanie zależności #

## Gemfile

    @@@ ruby
    gem 'krug_forum',
      :git => 'git@github.com:lucassus/krug_forum.git'

## config/application.rb

    @@@ ruby
    require 'krug_forum'

!SLIDE
# Podpięcie engine w routes #

## config/routes.rb

    @@@ ruby
    KrugForumDemo::Application.routes.draw do
      root :to => "index#index"
      mount KrugForum::Engine => "/forum"
    end

!SLIDE
# Migracje #

## Skopiowanie migracji do katalogu aplikacji

    @@@ sh
    rake my_forum:install:migrations

## ..i oczywiście ich uruchomienie

    @@@ sh
    rake db:migrate

!SLIDE title
# Pytania? #
