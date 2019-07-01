---
layout: post
title: Setup Rspec with Rails application
---

- Rspec-rails
- Shoulda-matchers
- Capybara
- Factory bot
- Database cleaner

# I. Setup Rspec
1. Add rspec-rails to the Gemfile

```
 echo 'gem "rspec-rails", :group => [:development, :test]' >> Gemfile
```

2. Install bundle

```
bundle install
```

3. Bootstrap Rspec

```
rails generate rspec:install
```

# II. Setup Shoulda Matchers - Simple One-liner tests for Rails
1. Add shoulda-matchers to the Gemfile

```
 echo 'gem "shoulda-matchers", :group => [:test]' >> Gemfile
```

2. Install bundle

```
bundle install
```

3. Config
- With Rails app

```
#spec/rails_hepler.rb
Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    with.test_framework :rspec
    with.library :rails
  end
end
```

# III. Setup Capybara
1. Add shoulda-matchers to the Gemfile

```
 echo 'gem "shoulda-matchers", :group => [:development, :test]' >> Gemfile
```

2. Install bundle

```
bundle install
```

3. Create spec/support/capybara.rb

```ruby
	# This file is copied to spec/ when you run 'rails generate rspec:install'
	require 'spec_helper'
	ENV['RAILS_ENV'] ||= 'test'
	require File.expand_path('../../config/environment', __FILE__)
	# Prevent database truncation if the environment is production
	abort("The Rails environment is running in production mode!") if Rails.env.production?
	require 'rspec/rails'
	# Add additional requires below this line. Rails is not loaded until this point!
	require 'support/capybara'
```

# IV. Setup Database cleaner
1. Add database-cleaner to the Gemfile

```
 echo 'gem "database-cleaner", :group => [:test]' >> Gemfile
```

2. Install bundle

```
bundle install
```

3. Create spec/support/database_clearner.rb

```ruby
Spec.configure do |config|

  config.use_transactional_fixtures = false
  
  config.before(:suite) do
    if config.use_transactional_fixtures?
      
      raise(<<-MSG)
        Delete line `config.use_transactional_fixtures = true` from rails_helper.rb
        (or set it to false) to prevent uncommitted transactions being used in
        JavaScript-dependent specs.
        During testing, the Ruby app server that the JavaScript browser driver
        connects to uses a different database connection to the database connection
        used by the spec.
        
        This Ruby app server database connection would not be able to see data that
        has been setup by the spec's database connection inside an uncommitted
        transaction.
        Disabling the use_transactional_fixtures setting helps avoid uncommitted
        transactions in JavaScript-dependent specs, meaning that the Ruby app server
        database connection can see any data set up by the specs.
      MSG

    end
  end
  
  config.before(:suite) do
    DatabaseCleaner.clean_with(:truncation)
  end

  config.before(:each) do
    DatabaseCleaner.strategy = :transaction
  end

  config.before(:each, type: :feature) do
    # :rack_test driver's Rack app under test shares database connection
    # with the specs, so we can use transaction strategy for speed.
    driver_shares_db_connection_with_specs = Capybara.current_driver == :rack_test

    if driver_shares_db_connection_with_specs
      DatabaseCleaner.strategy = :transaction
    else
      # Non-:rack_test driver is probably a driver for a JavaScript browser
      # with a Rack app under test that does *not* share a database 
      # connection with the specs, so we must use truncation strategy.
      DatabaseCleaner.strategy = :truncation
    end
  end

  config.before(:each) do
    DatabaseCleaner.start
  end

  config.after(:each) do
    DatabaseCleaner.clean
  end

end

```

# V. Setup Factory-bot
1. Add factory-bot-rails to the Gemfile

```
 echo 'gem "factory-bot-rails", :group => [:development, :test]' >> Gemfile
```

2. Install bundle

```
bundle install
```

3. Create spec/support/factory_bot.rb

```ruby
RSpec.configure do |config|
  config.include FactoryBot::Syntax::Methods

  config.before(:suite) do
    begin
      DatabaseCleaner.start
      # Test factories in spec/factories are working.
      FactoryBot.lint
    ensure
      DatabaseCleaner.clean
    end
  end

end
```

4. Create spec/factories folder
