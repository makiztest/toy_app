# Chapter 2 - A Toy App

##### LEARN WEB DEVELOPMENT WITH RAILS - *by Michael Hartl*

`I dont own any of the content of this. I'm just using this for educational purposes to teach myself about ruby on rails and i highly recommended it.`

To learn more about this please go to this link [Ruby on Rails Tutorial by Michael Hartl](https://www.railstutorial.org/book)

```scss
// TYPE IN TERMINAL
$ rails _5.1.4_ new toy_app
```

> generating the application skeleton using the rails new command with a specific Rails version number

---
`You should have the same Gemfile codes`
<details>
<summary>Gemfile.rb</summary>

```rb
# In /Gemfile.rb
source 'https://rubygems.org'

git_source(:github) do |repo_name|
  repo_name = "#{repo_name}/#{repo_name}" unless repo_name.include?("/")
  "https://github.com/#{repo_name}.git"
end

gem 'rails', '~> 5.1.4'
gem 'puma', '~> 3.7'
gem 'sass-rails', '~> 5.0'
gem 'uglifier', '>= 1.3.0'
# gem 'therubyracer', platforms: :ruby
gem 'coffee-rails', '~> 4.2'
gem 'turbolinks', '~> 5'
gem 'jbuilder', '~> 2.5'
# gem 'redis', '~> 3.0'
# gem 'bcrypt', '~> 3.1.7'
# gem 'capistrano-rails', group: :development
group :development, :test do
  gem 'sqlite3', '1.3.13'
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
  gem 'capybara', '~> 2.13'
  gem 'selenium-webdriver'
end

group :development do
  gem 'web-console', '>= 3.5.0'
  gem 'listen', '>= 3.0.5', '< 3.2'
  gem 'spring'
  gem 'spring-watcher-listen', '~> 2.0.0'
end

group :production do
  gem 'pg', '0.21.0'
end

gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]
```
> Next, we’ll use a text editor to update the Gemfile needed by Bundler
```rb
$ bundle install --without production
```
> We will install the local gems while suppressing the installation of production gems using the --without production option

</details>

---
`Now we’re ready to start` making the app itself. The typical first step when making a web application is to `create a data model`. the toy app will be a Twitter-style microblog.

## <u>Model for Users</u>
- Users of our toy app will have a unique identifier
    - `id` (of type integer)
    - a publicly viewable `name` (of type string)
    - an `email address` (also of type string) that will double as a unique username

`users` - table

|id   |integer|
|-----|-------|
|name |string |
|email|string |

> Think of users as objects that can be created, read, updated, and deleted through the web via the HTTP protocol

## <u>Model for microposts</u>
- A micropost has only an `id` and a `content field` for the micropost’s `text`

`microposts` - table

|id      |integer |
|--------|--------|
|content |text    |
|user_id |integer |

> The `user_id` attribute allows us to `express` the notion that a `user` potentially `has many` associated `microposts`.

## <u>Rails scaffolding</u>
```rb
# TYPE IN TERMINAL
$ rails generate scaffold User name:string email:string
```
> By including `name:string` and `email:string`, we have arranged for the User model to have the form.

Migrate the database created
```rb
# TYPE IN TERMINAL
$ rails db:migrate
```
## <u>Users URL - renders page</u>

|URL          |Action  |Purpose                        |
|-------------|--------|-------------------------------|
|/users       |index   |page to `list all users`       |
|/users/1     |show    |page to `show user with id 1`  |
|/users/new   |new     |page to `make a new user`      |
|/users/1/edit|edit    |page to `edit user` with `id 1`|

> Now we can create, edit, delete, and see users.

## <u>The Model-View-Controller (MVC) pattern</u>
- Here is the steps how it happens
1. The browser issues a request for the /users URL.
2. Rails routes /users to the index action in the Users controller.
3. The index action asks the User model to retrieve all users (User.all).
4. The User model pulls all the users from the database.
5. The User model returns the list of users to the controller.
6. The controller captures the users in the @users variable, which is passed to the index view.
8. The view uses embedded Ruby to render the page as HTML.
9. The controller passes the HTML back to the browser.3

```rb
# In config/routes.rb

#Rails.application.routes.draw do
#  resources :users
  root 'users#index'
#end
```
> Associate the root route with the users index, so that “slash” goes to /users
## <u>The Users controller in schematic form</u>
> the `index`, `show`, `new`, and `edit` actions render pages.

> `create`, `update`, and `destroy`. These actions don’t typically render pages (although they can); instead, their `main purpose is to modify information about users in the database`.
```rb
class UsersController < ApplicationController

  def index     # page to `list all users` 
  end

  def show      # page to `show user with :id`
  end

  def new       # page to make a `new user` 
  end

  def edit      # page to `edit user with :id`
  end 

  def create    # page to `create a new user`
  end

  def update    # page to `update user with :id`
  end

  def destroy   # page to `delete user with :id`
  end
end
```
 > This represents the implementation of the `REST architecture` in Rails.

 ## <u>`RESTful routes` provided `by the Users resource`</u>

|HTTP  |URL          |Action |Purpose                        |
|------|-------------|-------|-------------------------------|
|GET   |/users       |index  |page to `list all users`       |
|GET   |/users/1     |show   |page to `show user with id 1`  |
|GET   |/users/new   |new    |page to make a `new user`      |
|POST  |/users       |create |page to `create a new user`    |
|GET   |/users/1/edit|edit   |page to `edit user with id 1` |
|PATCH |/users/1     |update |page to `update user with id 1`|
|DELETE|/users/1     |destroy|page to `delete user with id 1`|

## <u>REpresentational State Transfer `(REST)`</u>

As a Rails application developer, the RESTful style of development helps you make choices about which controllers and actions to write: you simply structure the application using resources that get created, read, updated, and deleted

### Examine the relationship between the Users controller and the User model

```rb
# In app/controllers/users_controller.rb

class UsersController < ApplicationController
# ...

  def index
    @users = User.all
  end
  
# ...
end
```

 > This index action has the line `@users = User.all` which asks the User model to retrieve a list of all the users from the database, and then places them in the variable @users

```rb
# In app/models/user.rb

class User < ApplicationRecord
end
```
- Once the @users variable is defined, the controller calls the view. 
    - Variables that start with the @ sign, called instance variables, are automatically available in the views.

```rb
# In app/views/users/index.html.erb

# ...

<% @users.each do |user| %>
  <tr>
    <td><%= user.name %></td>
    <td><%= user.email %></td>
    <td><%= link_to 'Show', user %></td>
    <td><%= link_to 'Edit', edit_user_path(user) %></td>
    <td><%= link_to 'Destroy', user, method: :delete,
                                     data: { confirm: 'Are you sure?' } %></td>
  </tr>
<% end %>

# ...
```
> The view converts its contents to HTML, which is then returned by the controller to the browser for display

## <u>Weaknesses of this Users resource</u>

1. **No data validations**. Our User model accepts data such as blank names and invalid email addresses without complaint.

2. **No authentication**. We have no notion of logging in or out, and no way to prevent any user from performing any operation.

3. **No tests**. This isn’t technically true—the scaffolding includes rudimentary tests—but the generated tests don’t test for data validation, authentication, or any other custom requirements.
4. **No style or layout**. There is no consistent site styling or navigation.

5. **No real understanding**. If you understand the scaffold code, you probably shouldn’t be reading this book.

## <u>The Microposts resource</u>
This will be the same as what we did with the User scaffold.

```rb
# TYPE IN TERMINAL
$ rails generate scaffold Micropost content:text user_id:integer
```
Migrate the database created
```rb
# TYPE IN TERMINAL
$ rails db:migrate
```

> Now we are in a position to create microposts in the same way we created users.

> scaffold generator has updated the Rails routes file with a rule for Microposts resource.

## `RESTful routes` provided by `the Microposts resource`

|HTTP  |URL               |Action |Purpose                             |
|------|------------------|-------|------------------------------------|
|GET   |/microposts       |index  |page to `list all microposts`       |
|GET   |/microposts/1     |show   |page to `show micropost with id 1`  |
|GET   |/microposts/new   |new    |page to make a `new micropost`      |
|POST  |/microposts       |create |page to `create a new micropost`    |
|GET   |/microposts/1/edit|edit   |page to `edit micropost with id 1`  |
|PATCH |/microposts/1     |update |page to `update micropost with id 1`|
|DELETE|/microposts/1     |destroy|page to `delete micropost with id 1`|

> The `Micropost controller` will have a `similar schematic form like the User controller`.

> Go ahead and try the microposts routes and test it.

## <u>Putting the micro in microposts</u>

Implementing this constraint in Rails is easy with validations; to accept microposts with at most 140 characters (à la Twitter), we use a length validation.

```rb
# IN app/models/micropost.rb

class Micropost < ApplicationRecord
  validates :content, length: { maximum: 140 }
end
```
> If you create a content that is more than 140 in length you will get an error.

## A user `has_many` microposts

Rails has the ability to form associations between different data models.

```rb
# IN app/models/user.rb

class Micropost < ApplicationRecord
  has_many :microposts
end
```

```rb
# IN app/models/micropost.rb

class Micropost < ApplicationRecord
  belongs_to :user
  validates :content, length: { maximum: 140 }
end
```
- we can examine the implications of the user-micropost association by using the console.

```rb
# IN app/models/user.rb

class Micropost < ApplicationRecord
  has_may :microposts
  validates FILL_IN, presence: true    # Replace FILL_IN with the right code.
  validates FILL_IN, presence: true    # Replace FILL_IN with the right code.
end

# this should be our code by replacing the 'FILL_IN'

class Micropost < ApplicationRecord
  has_may :microposts
  validates :name, presence: true
  validates :email, presence: true
end
```

- Edit the user show page to display the content of the user’s first micropost

```rb
# IN app/controllers/users_controller.rb

class UsersController < ApplicationController
  before_action :set_user, only: [:show, :edit, :update, :destroy]
#...

  # find the user record
  @user = User.find(params[:id])
  @micropost = @user.microposts.first

#...
```
```rb
# TYPE IN A NEW TERMINAL

$ rails console or rails s
```
For now we are assigning manually the users to the microposts

```rb
# IN rails console TYPE

> first_user = User.first
```
We are assigning `first_user` as the `first user in our database`

```rb
# IN rails console TYPE

> first_user.microposts
```

```rb
> micropost = first_user.microposts.first
```

```rb
# IN rails console TYPE

> micropost.user
```

## <u>Inheritance hierarchies</u>

### `Model Inheritance`
- User model and the Micropost model inherit `(via the left angle bracket <)` from `ApplicationRecord`.

  - Which in turn inherits from `ActiveRecord::Base`, which is the base class for models provided by Active Record.

### `Controller Inheritance`
- The inheritance structure for controllers is essentially the same as that for models.

- Both the Users controller and the Microposts controller `inherit from the Application controller`.

- ApplicationController itself `inherits from ActionController::Base`.
  - which is the base class for controllers provided by the Rails library Action Pack.

## Deploying the toy app

```scss
// IN TERMINAL TYPE

$ heroku create
```

```scss
// IN TERMINAL TYPE

$ git push heroku master
```
To get the application’s database to work, you’ll also have to migrate the production database

```scss
// IN TERMINAL TYPE

$ heroku run rails db:migrate
```
> This updates the database at Heroku with the necessary user and micropost data models. After running the migration, you should be able to use the toy app in production, with a real PostgreSQL database back-end

## THE END