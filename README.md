# Circle.yml
Circle.yml
machine:

  timezone:
    Nairobi/Kenya 

  ruby:
    version:
      

  
  hosts:
    circlehost: 10.59.1.193
    
  environment:
    CIRCLE_ENV: test
    DATABASE_URL:
checkout:
  post:
    - git submodule sync
    - git submodule update --init # use submodules
dependencies:
  pre:
    - npm install coffeescript # install from a different package manager
    - gem uninstall bundler # use a custom version of bundler
    - gem install bundler --pre

  override:
    - bundle install: # note ':' here
        timeout: 180 # fail if command has no output for 3 minutes
        # IMPORTANT NOTE: ^^ the timeout modifier above must be
        # double indented (four spaces) from the previous line
  

  cache_directories:
    - "custom_1"   # relative to the build directory
    - "~/custom_2" # relative to the user's home directory


database:
  override:
  
    - cp config/database.yml.ci config/database.yml
    - bundle exec rake db:create db:schema:load


test:
  override:
    - phpunit test/unit-tests # use PHPunit for testing
  post:
    - bundle exec rake jasmine:ci: # add an extra test type
        environment:
          RAILS_ENV: test
          RACK_ENV: test


deployment:
  staging:
    branch: master
    heroku:
      appname: foo-bar-123


notify:
  webhooks:
