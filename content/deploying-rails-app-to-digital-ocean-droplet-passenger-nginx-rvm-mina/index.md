---
title: "Deploying Rails App to Digital Ocean Droplet. Passenger, Nginx, RVM, Mina"
description: "Working with ruby on rails is fun. But for a new developer setting up deployment becomes headache, if they miss some configuration. Heroku…"
date: "2023-02-12T08:55:10.704Z"
categories: []
published: false
---

Working with ruby on rails is fun. But for a new developer setting up deployment becomes headache, if they miss some configuration. [_Heroku_](https://www.heroku.com/) is great and easy for deploying rails application. But I do find some limitations using Heroku, like external files cannot be stored and also little expensive.

Digital Ocean gives more flexibility in terms of configuration and pricing. Recently I deployed an app on digital ocean, I thought I will also write a post explaining each step. Digital ocean already explains all these steps. But I do find some of the steps which are missing or difficult to find.

---

Let’s start!

[**CREATING A DROPLET**](https://www.digitalocean.com/docs/droplets/how-to/create/)

_Basically you will be signing into digital ocean, creating a new project and a droplet._ 

_For my project I have chosen these options_ 

undefinedundefinedundefined

It will take few seconds to setup. Once setup you should see an IP address!

undefined

---

[**Connecting to droplet from terminal using openSSH.**](https://www.digitalocean.com/docs/droplets/how-to/connect-with-ssh/openssh/)

From the terminal.

```
ssh root@your_ip_address
```

You should be able to login as a root user.

---

[**INITIAL SERVER SETUP**](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04)

Creating alternate user:

```
adduser appuser
```

You will be prompted to add password. Create a strong password. Add your new user to the _sudo_ group

```
usermod -aG sudo appuser
```

Pubkey Authentication:

If you do not already have an SSH key pair, which consists of a public and private key, you need to generate one.

Manually Installing the key:

On local machine:

```
cat ~/.ssh/id_rsa.pub
```

Select the public key, and copy it to your clipboard.

On the server, as the root user, enter the following command to temporarily switch to the new user.

```
su — appuser
```

Create a new directory called `.ssh` and restrict its permissions

```
mkdir ~/.ssh
chmod 700 ~/.ssh
```

open a file in `.ssh` called `authorized_keys` with a text editor

```
nano ~/.ssh/authorized_keys
```

Hit `CTRL-x` to exit the file, then `y` to save the changes that you made, then `ENTER` to confirm the file name.

restrict the permissions of the _authorized\_keys_

```
chmod 600 ~/.ssh/authorized_keys
exit
```

Now your public key is installed, and you can use SSH keys to log in as your user!

Test Login

Before you log out of the server, you should test your new configuration. Do not disconnect until you confirm that you can successfully log in via SSH.

In a new terminal on your local machine, log in to your server using the new account that you created. To do so, use this command (substitute your username and server IP address):

```
ssh appuser@your_server_ip
```

Setup a basic firewall

```
sudo ufw allow OpenSSH
sudo ufw enable
```

You can install any of the software you need on your server now. But you will need to install more firewalls later in the section

---

**Follow this tutorial to setup rvm and ruby:** [https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rvm-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rvm-on-ubuntu-16-04)

---

**Follow this tutorial to setup postgres:** [https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-16-04)

---

**Follow this tutorial to setup passenger and nginx:** [https://www.phusionpassenger.com/library/install/nginx/install/oss/xenial/](https://www.phusionpassenger.com/library/install/nginx/install/oss/xenial/)

**SAMPLE RAILS APP**

Create a sample project:

```
rails new mina_test -d postgresql
```

add gem “figaro” to gemfile.

```
bundle exec figaro install
```

It creates application.yml. Configure your ENV variables. SECRET\_KEY\_BASE is added in later section.

application.yml

Edit database.yml to match the ENV variables

database.yml

Changing app server from puma to passenger.

Edit Gemfile, replace puma to below line & run bundle:

```
gem "passenger", ">= 5.0.25", require: "phusion_passenger/rack_handler"
```

Create db and run locally:

```
rails db:create
bundle exec passenger start
```

application should be running on localhost:3000.

Simple scaffold:

```
rails g scaffold post
```

Change your routes.rb

routes.rb

Installing foreman

Add gem ‘foreman’ to gemfile & run bundle

Gemfile

Create \`Procfile\` in root folder

add passenger start command to Procfile. For adding more workers you can use this Procfile

Procfile```
foreman start
```

This should start the application on localhost:3000.

Creating secrets.yml

```
bundle exec rake secret
```

which outputs a secret key

secrets.yml

In application.yml add this with key as SECRET\_KEY\_BASE

```
chmod 700 config db
chmod 600 config/database.yml config/secrets.yml
```

---

### Configuring Nginx and Passenger on Server

ssh as appuser to server.

```
passenger-config about ruby-command
```undefined

take note of the path after “Command”.

```
sudo nano /etc/nginx/sites-enabled/mina_test
```mina\_test

change server name, root (we will discuss this later) and passenger\_ruby (this should be the path after command which you took a note of)

Restart nginx

```
sudo service nginx restart
exit
```

---

BITBUCKET CONFIGURATION

create a repository in your bitbucket account.

cd to your project on local machine:

```
git add .
git commit -m ‘initial commit’
git remote add origin git@bitbucket.org:urapp/mina_test.git
git push -u origin master
```

Also add application.yml file which is gitignored

```
git add — force — config/application.yml
git commit -m "add previously ignored files"
git push
```

Login to remote machine using appuser

setup git

```
sudo apt-get install -y git

sudo mkdir -p /var/www/mina_test
sudo chown appuser: /var/www/mina_test
```

Create the directory for serving the project. Path we specified in root path during configuration of \`sites-enabled/mina\_test\` and this path should be same.

Try to clone the repo from remote machine

```
cd /var/www/mina_test

sudo -u appuser -H git clone git@bitbucket.org:urapp/mina_test.git current
```

This gives an error.!! Because remote server doesn’t have access to the bitbucket repo.

To fix this, run:

```
ssh-keygen
```

which produces a public key:

run: 

```
cat ~/.ssh/id_rsa.pub
```

Which outputs a key. Copy the key. Go to bitbucket account. 

Repository

Inside Settings > Access keys

Click on \`Add key\`. and paste the generated key

Now run the git clone command. You should have your project inside current folder.!!. So the remote machine now has access to our repo.

Run \`bundle install\` inside current folder. If you get an error during pg native extension gem installation. First install:

```
sudo apt-get install libpq-dev
```

and then try again.

  

Remove the cloned project. We will deploy our app with the help of Mina.

run this command from \`/var/www/mina\_test\`

```
rm -rf current
```

  

### If you get an error during pg native extension gem installation.

  

Database migration and asset precompile on server.

bundle exec rake assets:precompile db:create db:migrate RAILS\_ENV=production

Run above command inside project folder. Production database will be created

  

Installing yarn

```
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update && sudo apt-get install yarn
```

  

  

**Deploying using Mina**

open gemfile add 

add gem ‘mina’

run: mina init

deploy.rb is added. add these settings.

```
require 'mina/bundler'

require 'mina/rails'

require 'mina/git'

require 'mina/rvm'    

set :application_name, 'mina_test'

set :domain, 'your_ip_address'

set :deploy_to, '/var/www/mina_test'

set :repository, 'git@bitbucket.org:urapp/mina_test.git'

set :user, 'appuser'

set :branch, 'master'

set :term_mode, nil

set :shared_paths, [

'config/database.yml',

'config/application.yml',

'config/secrets.yml',

'tmp',

'log'

]

task :remote_environment do

invoke :'rvm:use', 'ruby-2.4.1@mina_test'

end

task :setup do

command %[mkdir -p "#{fetch(:shared_paths)}/shared/log"]

command %[chmod g+rx,u+rwx "#{fetch(:shared_paths)}/shared/log"]

command %[mkdir -p "#{fetch(:shared_paths)}/shared/config"]

command %[chmod g+rx,u+rwx "#{fetch(:shared_paths)}/shared/config"]

command %[mkdir -p "#{fetch(:shared_paths)}/shared/tmp"]

command %[chmod g+rx,u+rwx "#{fetch(:shared_paths)}/shared/tmp"]

command %[touch "#{fetch(:shared_paths)}/shared/config/database.yml"]

command %[touch "#{fetch(:shared_paths)}/shared/config/application.yml"]

end

desc "Deploys the current version to the server."

task :deploy do

invoke :'git:ensure_pushed'

deploy do

invoke :'git:clone'

invoke :'deploy:link_shared_paths'

invoke :'bundle:install'

invoke :'rails:db_migrate'

invoke :'rails:assets_precompile'

invoke :'deploy:cleanup'

on :launch do

invoke :'passenger:restart'

in_path(fetch(:current_path)) do

command %{mkdir -p tmp/}

command %{touch tmp/restart.txt}

end

end

end

end

namespace :passenger do

desc "Restart Passenger"

task :restart do

command %{

echo "-----> Restarting passenger"

cd #{fetch(:shared_paths)}/current

#{echo_cmd %[mkdir -p tmp]}

#{echo_cmd %[touch tmp/restart.txt]}

}

end

end

```

  

From remote machine: 

```
curl -i http://localhost/422.html
```undefined

If you receive 200 OK. Your nginx is serving properly. But if you curl from your local machine. You will not get. You will get timeout. Because of firewall. Your remote machine denies all incoming requests. You need to set up your firewall properly.

Follow this tutorial to setup firewall: [https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-16-04)
