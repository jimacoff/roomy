# Roomy
> A fully responsive P2P room booking app

This web platform enables you to **search**, **book or advertise your room**. Anytime and anywhere!

Technology Stack: Ruby on Rails, Bootstrap, JavaScript/jQuery, SQLite DB

## Prerequisites
- Make sure, to have [Ruby installed](https://www.ruby-lang.org/en/documentation/installation/).
- To handle different Ruby versions on the same machine, I recommend installing a Ruby version manager like [rbenv](https://github.com/rbenv/).

## Running the Application
- Fork the application
- Clone your fork to your machine via ```git clone link_to_forked_repo_here```
- _Optional_: add this repo as upstream via ```git remote add upstream https://github.com/togiberlin/roomy.git```
- Check remote repos ```origin``` and ```upstream``` via ```git remote -v```
- Type ```bundle install``` to install dependencies
- Then enter ```rackup private_pub.ru -s thin -E production``` to allow some WebSocket real-time messaging magic to happen
- Check the ```Transactional Emails``` and ```PayPal Integration``` sections below
- In the last step, type ```bundle exec rails s``` to start the Rails server
- Open your browser on ```http://localhost:3000```

## Extra Features
### Avatar
- Simply go to [Gravatar](https://www.gravatar.com) and register. 
- Gravatar is a service which lets you use a single avatar across multiple pages, e.g. GitHub, DisQus, Wordpress, Stackoverflow and more. 
- When registering, make sure to use the same Gravatar account email.

### Transactional E-Mails
- Transactional e-mails such as registration, account confirmation etc. are generated by the gem [Devise](https://github.com/plataformatec/devise)
- Sending the automatically generated e-mail via SMTP can be done by e.g. Sendgrid
- Go to [Sendgrid](http://www.sendgrid.com) to register a new account with a free plan (30-day free trial)
- Sendgrid will ask for some personal details. Make sure to fill them out.
- Make sure to click on the confirmation email sent by SendGrid
- Go to ```roomy/config/application_example.yml``` and insert your Sendgrid username and password. Then, remove the ```_example``` from the filename. In the end, the filename should look like this: ```roomy/config/application.yml```. This file is now on ```.gitignore```.
- If you chose to skip this step, you can simply just find the confirmation link inside the Rails logs. Copy/paste the confirmation link into the browser, and the account is good to go!

### Login with Facebook
- Go to [developer.facebook.com](https://developer.facebook.com) and register
- Create an app called "Roomy"
- Add ```http://localhost:3000``` to the URL whitelist
- Copy and paste your app-ID and app secret into ```roomy/config/application.yml```

### PayPal Integration
- Because our application runs on ```http://localhost:3000```, there is no way that PayPal can send a payment confirmation to our app.
- There are 2 solutions. Deployment to e.g. Heroku, which requires some work. Way easier: using a webservice that allows connections to your localhost webapp.
- In order to allow connections to your localhost app, download Ngrok from [here](https://ngrok.com/)
- Unzip the file, right click on it and open it with Terminal. A ```complete``` message should appear
- If you don't have a PayPal account, register and open up a personal account [here](https://www.paypal.com)
- Go to [developer.paypal.com](https://developer.paypal.com) and login
- Create an app called ```Roomy```
- Go to the Sandbox test accounts page. There should be 2 accounts: one facilitator (business) and one buyer (private) account.
- Click on ```profile``` on the private account and change the password. Make sure, that the buyer account has a balance of 9999 EUR/USD
- Navigate to the download folder via ```cd ~/Downloads```, then enter ```./ngrok http 3000``` to start the Ngrok server
- Ngrok displays now a link, with which the internet can reach your localhost application. Start your Rails server, and copy/paste the Ngrok link into the browser. Your webapp should appear on the internet!
- Make sure to add all configuration stuff into the ```roomy/config/application.yml``` file. It should look like this:
```
# PayPal integration
pp_facilitator_email: 'johndoe-facilitator@hotmail.com'
ngrok_notify_link: 'http://fs3442jd.ngrok.io/notify'
ngrok_return_link: 'http://fs3442jd.ngrok.io/show_trips'
```

## Cleaning the DB
If you want to reset the database, simply run these commands
```
bundle exec rake db:drop        # delete entire SQLite database
bundle exec rake db:create      # create a new SQLite database
bundle exec rake db:migrate     # run migrations
bundle exec rake db:seed        # fill the DB with test users, rooms and room photos
```

## Debugging
If you want to create a breakpoint, simply enter:
```
binding.pry
```
You can now jump through the code via
```
continue     # continue until next breakpoint
next         # go to next line of code
abort        # stop debugging
```

## Improvement Points
The MVP is there, but there are improvement points.

- Design is currently based on Bootstrap. Redesigning the UI and giving the web app more of an individual look would be nice.
- Replacing all strings inside the view with ```I18n``` placeholder. Store all strings inside dedicated Yaml file. Localization into different languages is then very easy.
- Replacing ```Test::Unit``` with ```RSpec``` for more readable and documented tests.  Write tests, where at least 90% of the code is triggered (high code coverage).
- Introducing ```Capybara``` and ```Cucumber``` for UI-testing and UI-test documentation.
- Add badges for static code analysis tools. E.g. syntax-check, style, security, code duplication etc.
- Continuos Integration. Automately trigger tests with e.g. ```Travis CI```. All future pull requests muss pass all tests before merging. Green build-status badge is requirement for merging.
- Deploy under any environment without weird anomalies. Creating a docker image for easier deployment would be nice.
- With that Docker image, automate the process of deployment via e.g. ```Puppet```, ```Chef```, ```Ansible``` or ```Jenkins```.
- Don't commit on ```master``` anymore. Create a ```develop``` branch. Work with pull requests. In short: use a proper [branching strategy](http://nvie.com/posts/a-successful-git-branching-model/). Make code reviews necessary.
- In production, serve static content with ```Nginx``` to avoid unnecessary load.
- App currently uses Rails version ```4.2.5```. Update to Rails 5 would be cool.
- Make use of Turbolinks to avoid full page reloads.
- Or even better, replace frontend with ```React``` and ```Redux``` to outsource more logic (and therefore load) into the client.