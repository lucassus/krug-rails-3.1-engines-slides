!SLIDE bullets
# Struktura katalogów #

* krug_forum.gemspec - pozwala na użycie engine jako gema
* Gemfile - zostanie skonfigurowany tak by używac gemspeca
* config/routes.rb - routes enginu
* script/rails - server, generatory ipt.

!SLIDE bullets
# Struktura katalogu app/ #

* tak jak w standardowej aplikacji rails
* kontrolery, widoki, modele, helpery itp.
* ..ale są przestrzenie nazw

!SLIDE bullets
# Plik lib/krug_forum.rb #

* definiuje moduł KrugForum
* można tutaj umieszczać helpery
* ..np. konfiguracja engine

!SLIDE bullets
# Plik lib/krug_forum/engine.rb #

* definiuje klasę KrugForum::Engine
* dziedziczy po Rails::Engine
* punkt styku pomiędzy engine a aplikacją rails

!SLIDE bullets
# Struktura test/dummy #

* najbardziej zabawne część engine
* mini aplikacja Rails
* w zależnościach ma nasz engine
* używana do uruchamiania testów
