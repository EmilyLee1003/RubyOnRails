To the Hiring Manager:

<br />
     I learned Ruby on Rails and Postgresql for this take home project. There are some errors that I have encountered that I could not fix(please refer to the home page) but I also have implemented other features I have worked with before. I believe that with the help of a mentor and some extra time I can finish this project to completion. Most of all I had fun working on this project and plan on adding moree complexity in the future. Here are some screenshots to briefly look at the front end of my application.
 <br />
 <br />
<img width="1436" alt="Screen Shot 2022-07-06 at 9 22 52 AM" src="https://user-images.githubusercontent.com/64618285/177598101-9b5ce0b3-0e33-425a-9098-e0265a394051.png">

<img width="1436" alt="Screen Shot 2022-07-06 at 9 23 11 AM" src="https://user-images.githubusercontent.com/64618285/177598086-4c8b6ef9-fedb-4618-8777-d7d8c5bbc39b.png">

<img width="1438" alt="Screen Shot 2022-07-06 at 9 23 24 AM" src="https://user-images.githubusercontent.com/64618285/177598075-e4ff0949-b496-46c9-a408-c907ace866dd.png">

<img width="560" alt="Screen Shot 2022-07-06 at 9 27 57 AM" src="https://user-images.githubusercontent.com/64618285/177599172-9122767d-30db-45b6-8598-cbe3576dfc15.png">

<img width="474" alt="Screen Shot 2022-07-06 at 9 26 26 AM" src="https://user-images.githubusercontent.com/64618285/177598528-928cd8db-51d3-40dc-9aea-38d825ef6033.png">


# Getting started
To get the Rails server running locally:

- Clone this repo
- `bundle install` to install all req'd dependencies
- `rake db:migrate` to make all database migrations
- `rails s` to start the local server

# Code Overview

## Dependencies

- [acts_as_follower](https://github.com/tcocca/acts_as_follower) - For implementing followers/following
- [acts_as_taggable](https://github.com/mbleigh/acts-as-taggable-on) - For implementing tagging functionality
- [Devise](https://github.com/plataformatec/devise) - For implementing authentication
- [Jbuilder](https://github.com/rails/jbuilder) - Default JSON rendering gem that ships with Rails, used for making reusable templates for JSON output.
- [JWT](https://github.com/jwt/ruby-jwt) - For generating and validating JWTs for authentication

## Folders

- `app/models` - Contains the database models for the application where we can define methods, validations, queries, and relations to other models.
- `app/views` - Contains templates for generating the JSON output for the API
- `app/controllers` - Contains the controllers where requests are routed to their actions, where we find and manipulate our models and return them for the views to render.
- `config` - Contains configuration files for our Rails application and for our database, along with an `initializers` folder for scripts that get run on boot.
- `db` - Contains the migrations needed to create our database schema.

## Configuration

### camelCase Payloads

- [`config/initializers/jbuilder.rb`](https://github.com/gothinkster/rails-realworld-example-app/blob/master/config/initializers/jbuilder.rb) - Jbuilder configuration for camelCase output
- [`app/controllers/application_controller.rb#underscore_params!`](https://github.com/gothinkster/rails-realworld-example-app/blob/master/app/controllers/application_controller.rb#L44) - Convert camelCase params into snake_case params

### null_session

By default Ruby on Rails will throw an exception when a request doesn't contain a valid CSRF token. Since we're using JWT's to authenticate users instead of sessions, we can tell Rails to use an empty session instead of throwing an exception for requests by specifying `:null_session` in [app/controllers/application_controller.rb](https://github.com/gothinkster/rails-realworld-example-app/blob/master/app/controllers/application_controller.rb#L4).

### Authentication

Requests are authenticated using the `Authorization` header with a valid JWT. The [application_controller.rb#authenticate_user!](https://github.com/gothinkster/rails-realworld-example-app/blob/master/app/controllers/application_controller.rb#L32) filter is used like the one provided by Devise, it will respond with a 401 status code if the request requires authentication that hasn't been provided. The [application_controller.rb#authenticate_user](https://github.com/gothinkster/rails-realworld-example-app/blob/master/app/controllers/application_controller.rb#L18) filter is called on every request to try and authenticate the `Authorization` header. It will only interrupt the request if a JWT is present and invalid. The user's id is then parsed from the JWT and stored in an instance variable called [`@current_user_id`](https://github.com/gothinkster/rails-realworld-example-app/blob/master/app/controllers/application_controller.rb#L24). `@current_user_id` can be used in any controller when we only need the user's id to save a trip to the database. Otherwise, we can call [`current_user`](https://github.com/gothinkster/rails-realworld-example-app/blob/master/app/controllers/application_controller.rb#L36) to fetch the authenticated user from the database.

Devise only requires an email and password upon registration. To allow additional parameters on sign up, we use [application_controller#configure_permitted_parameters](https://github.com/gothinkster/rails-realworld-example-app/blob/master/app/controllers/application_controller.rb#L14) to allow additional parameters.
