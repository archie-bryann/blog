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
