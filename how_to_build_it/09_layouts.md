!SLIDE title subsection
# Layouty i widoki #

!SLIDE small
# Konfigurowalny layout #

## lib/krug_forum.rb

    @@@ ruby
    # ...
    DEFAULT_LAYOUT = 'krug_forum/application'

    def self.layout(layout = nil)
      @layout = layout if layout
      @layout || DEFAULT_LAYOUT
    end
    # ...

!SLIDE small
## app/controllers/krug_forum/application_controller.rb

    @@@ ruby
    module KrugForum
      class ApplicationController < ActionController::Base
        layout KrugForum.layout

        # ...
      end
    end

!SLIDE
# W głównej aplikacji #

## config/initializers/krug_forum.rb

    @@@ ruby
    KrugForum.layout 'application'

!SLIDE smaller
# Uwaga! #

## `main_app`
## `krug_forum`

    @@@ html
    <body>

      <div id="menu">
        <%= link_to 'Application root', main_app.root_path %> |
        <%= link_to 'Engine root', krug_forum.root_path %> |

        <% if user_signed_in? %>
          Email: <%= current_user.email %> |
          <%= link_to 'Logout', destroy_user_session_path %>
        <% else %>
          <%= link_to 'Login', main_app.new_user_session_path %> |
          <%= link_to 'Register', main_app.new_user_registration_path %>
        <% end %>
      </div>

      <%= yield %>

    </body>

