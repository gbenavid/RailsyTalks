Haml-rails provides Haml generators for Rails 4. It also enables Haml as the templating engine for you, so you don't have to screw around in your own application.rb when your Gemfile already clearly indicated what templating engine you have installed. 


to install:
```
gem "haml-rails", "~> 0.9"
```

* you generate a resource, view, or mailer, you'll get Haml templates (instead of ERB)
* your Rails application loads, Haml will be loaded and initialized automatically
* Haml templates will be respected by the view template cache digestor


# Next
Convert the .erb layout fifle by using the command:
```
$ rails generate haml:application_layout convert
```
# Gotcha (maybe)
delete the file: `app/views/layouts/application.html.erb` 
so your application can start using the new file


### convert all of your erb files to haml
$ rake haml:erb2haml


Sources:
https://github.com/indirect/haml-rails

Thank you!