# README

# Seeding the database

1. Open `db/seeds.rb` and add the following code:

```ruby
Post.create([
    { title: "First Post", body: "This is the body of the first post." },
    { title: "Second Post", body: "Here is some more body for the second post." },
    { title: "Third Post", body: "This post contains some interesting insights." },
    { title: "Fourth Post", body: "This body of the fourth post is quite engaging." },
    { title: "Fifth Post", body: "This is the fifth post with exciting information!" }
])
```

2. Run the seed command in your terminal to populate the database:

```
rails db:seed
```

# Create a migration

```
rails g migration AddUserIdToPosts user_id:integers
```

# Start rails server on a custom port

```
rails s -p 4000
```

# Creating a custom route for a custom view and controller

Open `config/routes.rb` and add the following code:

```ruby
get 'myposts' as: 'posts/myposts' to: 'posts#myposts'
```

- **get** → defines the HTTP method and URL path (e.g. `GET /myposts`)
- **as** → sets the internal helper name used for linking (e.g. `myposts_path`, `myposts_url`)
- **to** → points to the controller and action that handle the request (e.g. `posts#myposts`)

# Drop the database

```
rails db:drop:_unsafe
```

# Create the database

```
rails db:create
```

# Add a username to registration

1. Added field `user_name` to schema.rb via migration

2. Generate devise controllers

Since, we want to be able to enable authentication with `username`:

```
rails g devise:controllers users
```

3. Update `config/routes.rb` per instructions from the terminal.

4. Open `controllers/registrations_controller` & edit what sign up params to permit in `configure_signup_params`, of course, after uncommenting the relating `before_action`.

5. Add it in the form.

# Add a unique field to DB

```
rails g migration AddSlugToPosts slug:uniq
```

# Create a custom page with functionality

1. Generate a user controller with a method & view

```
rails g controller users show
```

2. Add functionality to the show controller

```ruby
@user = User.find(params[:id])
```

3. Access `@user` in the `show` view.

# Ransack guide

https://activerecord-hackery.github.io/ransack/getting-started/simple-mode/

1. Install the gem

2. Add below to controller index

```ruby
@q = Post.ransack(params[:q])
@posts = @q.result(distinct: true)
```

Searching for a post overwriting the previous value of `@posts`, the default value being all posts.

3. Add below to the `.html.erb` file

```erb
<%= search_form_for @q, url: posts_path, method: :get do |f| %>
  <div>
    <%= f.label :title_cont, "Search by Title" %>
    <%= f.search_field :title_cont, placeholder: "Enter title" %>
  </div>
  <div>
    <%= f.submit "Search" %>
  </div>
<% end %>
```

4. Add the following to the model

```ruby
def self.ransackable_attributes(auth_object = nill)
    ["title"]
end
```

# Setting up postgreSQL

1. Update Linux

```
sudo apt update
```

2. Install PostgreSQL

```
sudo apt install postgresql postgresql-contrib libpq-dev
```

3. Update `config/database.yml` with below:

```yaml
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: postgres
  password: Rails.application.credentials.database_password
  host: localhost

development:
  <<: *default
  database: myapp

test:
  <<: *default
  database: myapp_test

production:
  <<: *default
  database: myapp_production
```

4. Open the CLI

Run:

```
export EDITOR=nano
```

and then run:

```
rails credentials:edit
```

5. Add below to the opened `credentials.yml` file, then save and exit:

```yml
database_password: 12345678
```

6. Set a password for the `postgres` User

Switch to the `postgres` user (which is the default):

```
sudo -i -u postgres
```

Access the PostgreSQL command line:

```
psql
```

Set a password for the `postgres` user:

```
ALTER USER postgres PASSWORD '12345678'
```

Then exit by running the following:

```
\q
```

```
exit
```

7. Then drop the db & create a new one

```
rails db:drop:_unsafe
```

```
rails db:create
```

Ran into errors, where I couldn't login with the default `postgres`, so the workaround is to simply repeat step 6 above, but then run the following command in the PostgresSQL command line:

```
CREATE USER shayon WITH PASSWORD 'shayon';
```

```
ALTER USER shayon WITH SUPERUSER;
```

Replace with preferred username and password.

Source: https://stackoverflow.com/a/70240100

# Setting up postgreSQL with .env (ChatGPT)

1. Add dotenv support

In your Gemfile (inside the :development, :test group is enough, but you can also make it global):

```
gem "dotenv-rails"
```

Then run:

```
bundle install
```

2. Create a .env file

```dotenv
PG_USERNAME=myuser
PG_PASSWORD=mypassword
PG_HOST=localhost
PG_PORT=5432
PG_DATABASE=myapp_db
```

3. Your database.yml (already set up from before) will pick these up

```yaml
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: <%= ENV.fetch("PG_USERNAME") { "myuser" } %>
  password: <%= ENV.fetch("PG_PASSWORD") { "mypassword" } %>
  host: <%= ENV.fetch("PG_HOST") { "localhost" } %>
  port: <%= ENV.fetch("PG_PORT") { 5432 } %>
  database: <%= ENV.fetch("PG_DATABASE") { "myapp_db" } %>

development:
  <<: *default
  database: myapp_development

test:
  <<: *default
  database: myapp_test

production:
  <<: *default
  database: myapp_production
```

4. Once that’s set up, you can run:

```
rails db:create db:migrate
```
