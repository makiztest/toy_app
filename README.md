# Chapter 2 - A Toy App
```scss
// TYPE IN TERMINAL
$ rails _5.1.4_ new toy_app
```

> generating the application skeleton using the rails new command with a specific Rails version number

---
You should have the same Gemfile codes
<details>
<summary>Gemfile.rb</summary>

```rb
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
## Model for Users
- Users of our toy app will have a unique identifier
    - `id` (of type integer)
    - a publicly viewable `name` (of type string)
    - an `email address` (also of type string) that will double as a unique username

`users` - table

|users|
|-------------|
|id   |integer|
|-----|-------|
|name |string |
|email|string |

