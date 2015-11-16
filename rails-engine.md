Rails-4 engine with rspec-rails, capybara and factory_girl_rails
-----------------------------------------------------------------

1. Create our engine with name ``blorgh``

    $ rails plugin new blorgh --mountable -T --dummy-path=spec/dummy

2. Next step is to add the ``rspec-rails`` gem to our ``gemspec``

    ```ruby
    # blorgh.gemspec

    s.test_files = Dir["spec/**/*"]

    s.add_development_dependency "rspec-rails", '~> 3.0'
    s.add_development_dependency "capybara"
    s.add_development_dependency "factory_girl_rails"
    ```

3. Install and configure rspec-rails

    ```bash
    $ bundle install
    $ rails g rspec:install
    ```

4. It will generate rspec configuration files for our engine. Make below changes in your ``spec/rails_helper.rb``

    ```ruby

    # This file is copied to spec/ when you run 'rails generate rspec:install'
    ENV['RAILS_ENV'] ||= 'test'
    require 'spec_helper'

    #########################################
    # Change below line of code:
    # require File.expand_path("../../config/environment", __FILE__)
    # TO:
    require File.expand_path("../dummy/config/environment", __FILE__)
    #########################################
    
    require 'rspec/rails'
    require 'factory_girl_rails'
    # Add additional requires below this line. Rails is not loaded until this point!

    # Requires supporting ruby files with custom matchers and macros, etc, in
    # spec/support/ and its subdirectories. Files matching `spec/**/*_spec.rb` are
    # run as spec files by default. This means that files in spec/support that end
    # in _spec.rb will both be required and run as specs, causing the specs to be
    # run twice. It is recommended that you do not name files matching this glob to
    # end with _spec.rb. You can configure this pattern with the --pattern
    # option on the command line or in ~/.rspec, .rspec or `.rspec-local`.
    #
    # The following line is provided for convenience purposes. It has the downside
    # of increasing the boot-up time by auto-requiring all files in the support
    # directory. Alternatively, in the individual `*_spec.rb` files, manually
    # require only the support files necessary.
    #
    #########################################
    # change below line of code:
    # Dir[Rails.root.join('spec/support/**/*.rb')].each { |f| require f }
    # TO:
    Dir["#{File.dirname(__FILE__)}/support/**/*.rb"].each { |f| require f }
    #########################################


    # Checks for pending migrations before tests are run.
    # If you are not using ActiveRecord, you can remove this line.
    ActiveRecord::Migration.maintain_test_schema!

    RSpec.configure do |config|
      # Remove this line if you're not using ActiveRecord or ActiveRecord fixtures
      config.fixture_path = "#{::Rails.root}/spec/fixtures"

      # If you're not using ActiveRecord, or you'd prefer not to run each of your
      # examples within a transaction, remove the following line or assign false
      # instead of true.
      config.use_transactional_fixtures = true

      # RSpec Rails can automatically mix in different behaviours to your tests
      # based on their file location, for example enabling you to call `get` and
      # `post` in specs under `spec/controllers`.
      #
      # You can disable this behaviour by removing the line below, and instead
      # explicitly tag your specs with their type, e.g.:
      #
      #     RSpec.describe UsersController, :type => :controller do
      #       # ...
      #     end
      #
      # The different available types are documented in the features, such as in
      # https://relishapp.com/rspec/rspec-rails/docs
      config.infer_spec_type_from_file_location!
    end


    

    ```

5. Tell our engine to use rspec as a default test framework

    ```ruby
    # lib/blorgh/engine.rb

    module Blorgh
      class Engine < ::Rails::Engine
        isolate_namespace Blorgh

        # added lines
        config.generators do  |g|
          g.test_framework :rspec,              fixture: false
          g.fixture_replacement :factory_girl,  dir: 'spec/factories'
          g.assets false
          g.helper false
        end
      end
    end

    ```

6. Now we are almost done with `rspec` and `factory_girl` configuration. So use rails ``scaffold`` generator to generate `article` resource like below:

    ```
      $ rails g scaffold article title:string body:text
      invoke  active_record
      create    db/migrate/20150525084612_create_blorgh_articles.rb
      create    app/models/blorgh/article.rb
      invoke    rspec
      create      spec/models/blorgh/article_spec.rb
      invoke      factory_girl
      create        spec/factories/blorgh_articles.rb
      invoke  resource_route
       route    resources :articles
      invoke  scaffold_controller
      create    app/controllers/blorgh/articles_controller.rb
      invoke    erb
      create      app/views/blorgh/articles
      create      app/views/blorgh/articles/index.html.erb
      create      app/views/blorgh/articles/edit.html.erb
      create      app/views/blorgh/articles/show.html.erb
      create      app/views/blorgh/articles/new.html.erb
      create      app/views/blorgh/articles/_form.html.erb
      invoke    rspec
      create      spec/controllers/blorgh/articles_controller_spec.rb
      create      spec/views/blorgh/articles/edit.html.erb_spec.rb
      create      spec/views/blorgh/articles/index.html.erb_spec.rb
      create      spec/views/blorgh/articles/new.html.erb_spec.rb
      create      spec/views/blorgh/articles/show.html.erb_spec.rb
      create      spec/routing/blorgh/articles_routing_spec.rb
      invoke      test_unit
      create        test/integration/blorgh/article_test.rb

    ```




TO BE CONTINED .....