# Pagination with Pagy Demo

This demonstration shows how to use the [Pagy](https://github.com/ddnexus/pagy#readme) gem to add pagination to the todos index page from the [Index Pages Demo](https://rails-demos-n-deets-2023.herokuapp.com/demos/index-pages).

If you would like to follow along with the video, clone this repo and switch to the `version-before-demo` branch. The `main` (default) branch holds the solution.

## Video Demo (16 minutes)

[![Screenshot of todos index page with pagination buttons at the bottom of the page](todos_index_with_pagination.png)](https://youtu.be/jRvoGUCMzhM?si=XGGdh_7N1JbPBrEn)

### [🎦 Watch the Video Demo](https://youtu.be/jRvoGUCMzhM?si=XGGdh_7N1JbPBrEn)

## Steps to Generate 1000 Todo Seeds

### ① Install Faker Gem

In the `Gemfile`, insert `gem 'faker'` at the bottom. Then `bundle install`.

### ② Load Faker Gem in Seeds Script

In `db/seeds.rb`, insert `require 'faker'` at the top of the file.

### ③ Create 1000 Random Todos in Seeds Script

In `db/seeds.rb`, insert a loop that creates 1000 `Todo` objects and that uses Faker to initialize each object with random attribute values.

```ruby
1000.times do
  Todo.create!(
    title:       Faker::Lorem.sentence,
    description: Faker::Lorem.paragraph,
    due_date:    Faker::Date.between(from: 1.year.ago, to: 1.year.from_now)
  )
end
```

### ④ Reset and Reseed the DB

Run `rails db:migrate:reset && rails db:seed` to reset and reseed the database.

## Steps to Install and Set Up Pagy

### ① Install Pagy Gem

In the `Gemfile`, insert `gem 'pagy', '~> 9.3'` at the bottom. Then `bundle install`.

### ② Download Pagy Config File

Download `pagy.rb` from <https://ddnexus.github.io/pagy/gem/config/pagy.rb> and save the file in `config/initializers`.

### ③ Load Pagy Bootstrap Extras

In `config/initializers/pagy.rb`, uncomment the line `require 'pagy/extras/bootstrap'`.

### ④ Include Pagy Backend Module in Controllers

In `app/controllers/application_controller.rb`, include the `Pagy::Backend` module in the `ApplicationController` class.

```ruby
class ApplicationController < ActionController::Base
  include Pagy::Backend
end
```

### ⑤ Include Pagy Frontend Module in Views

In `app/helpers/application_helper.rb`, include the `Pagy::Frontend` module in the `ApplicationHelper` module.

```ruby
module ApplicationHelper
  include Pagy::Frontend
end
```

## Steps to Add Pagination to the Todos Index Page (10 per Page)

### ① Use Pagy in Index Controller Action

In `app/controllers/todos_controller.rb`, in the `index` action, update the statement that retrieves the `Todo` objects to use Pagy.

```ruby
@pagy, @todos = pagy(Todo.order(:due_date), limit: 10)
```

### ② Add Page Nav Buttons to Index View

- **Step 2:** In `app/views/todos/index.html.erb`, insert the page navigation buttons after the table.

```erb
<%== pagy_bootstrap_nav(@pagy, classes: 'pagination mx-auto') if @pagy.pages > 1 %>
```

## Further Reading

- [Faker Homepage](https://github.com/faker-ruby/faker#readme)
- [Faker Documentation](https://www.rubydoc.info/gems/faker/)
- [Pagy Homepage](https://github.com/ddnexus/pagy#readme)
- [Pagy Documentation](https://ddnexus.github.io/pagy/)
