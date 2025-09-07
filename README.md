# README

# Seeding the database

1. Add the seed in `db/seeds.rb`:

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
