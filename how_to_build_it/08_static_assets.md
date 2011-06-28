!SLIDE title
# Serwowanie plików js i css #
## (na przykładzie jquery-rails) ##

!SLIDE bullets incremental
# Instalacja zależności #

* dodać `s.add_dependency 'jquery-rails'` do __krug_forum.gemspec__
* ..i nie zapomnieć o `require 'jquery-rails'` w __lib/krug_forum/engine.rb__

!SLIDE
# Podpięcie assets w aplikacji #

## app/assets/javascripts/krug_forum.js

    @@@ javascript
    //= require jquery
    //= require jquery_ujs

## spec/dummy/assets/javascript/application.js

    @@@ javascript
    //= require krug_forum

