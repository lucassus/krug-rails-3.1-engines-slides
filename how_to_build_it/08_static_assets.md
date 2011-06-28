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

