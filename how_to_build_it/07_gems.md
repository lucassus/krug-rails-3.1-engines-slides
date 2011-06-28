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

!SLIDE bullets incremental small
# Dodawanie użytkowników #

* do __Gemfile__ dodać `gem 'devise'`
* zainstalować devise, w katalogu __spec/dummy__:
* `rails generate devise:install`
* `rails generate devise User`
* migracja w __spec/dummy/db/migrate__

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
          base.has_many :krug_forum_posts, 
            :class_name => 'KrugForum::Post'

          base.send(:include, InstanceMethods)
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

## spec/dummy/app/models/user.rb

    @@@ ruby
    class User < ActiveRecord::Base
      include KrugForum::Extensions::User
    end

