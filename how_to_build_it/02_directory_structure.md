!SLIDE bullets incremental
# Struktura katalogów #

* __krug_forum.gemspec__ - pozwala na użycie engine jako gema
* __Gemfile__ - zostanie skonfigurowany tak by używac gemspeca
* __config/routes.rb__ - routes enginu
* __script/rails__ - serwer, generatory ipt.

!SLIDE bullets
# Struktura katalogu app/ #

* Tak jak w standardowej aplikacji rails
* mamy __kontrolery, widoki, modele, helpery__ itp.
* ..ale są też __przestrzenie nazw__

!SLIDE bullets
# Plik lib/krug_forum.rb #

* Definiuje moduł __KrugForum__
* Można tutaj umieszczać helpery
* ..np. konfiguracja engine itp.

!SLIDE bullets
# Plik lib/krug_forum/engine.rb #

* Definiuje klasę __KrugForum::Engine__
* Dziedziczy po __Rails::Engine__
* Punkt styku pomiędzy engine a aplikacją rails

!SLIDE bullets incremental
# Struktura test/dummy #

* Najbardziej zabawna część engine
* Mini aplikacja Rails
* W zależnościach ma nasz engine
* Używana do uruchamiania testów
* ..lub po prostu aplikacji `rails server`
