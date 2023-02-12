---
title: "React, Redux, Redux-Saga, Reselect, Rails: Part 1"
description: "There are at least four ways for creating apps using react and rails."
date: "2023-02-12T08:55:10.840Z"
categories: []
published: false
---

There are at least four ways for creating apps using react and rails.

1.  [react\_on\_rails](https://github.com/shakacode/react_on_rails)
2.  [react\_rails gem](https://github.com/reactjs/react-rails)
3.  [ruby-hyperloop](http://ruby-hyperloop.org/)
4.  Ruby on Rails API with React as frontend

react-rails & react-on-rails gems are the simplest way to get started with react in a rails application. Ruby-hyperloop allows to create react components using ruby.

I decided to write this series, because of several reasons. We started building the product using react\_on\_rails gem. Which is by far the simplest way to get started. But I feel that it is always a good thing to separate backend from frontend.

So I will be using react as front end and rails as backend api.

Setting up rails api

open the terminal

```
rails new railsApi --api
```

Add the below line in config/environments/development.rb

```
config.debug_exception_response_format = :api
```

Which preserves the response format when error occurs in development mode.

Installing foreman

```
gem install foreman
```

Add Procfile

```
touch Procfile
```

and paste

```
web: bundle exec puma -C config/puma.rb
```

type

```
foreman start
```

That should set up rails for development mode

Push to heroku

```
heroku login
heroku create
```

Open Gemfile

change sqlite to pg

Open database.yml

change adapter to postgresql

change database name

```
git add .
git commit -m 'db setup'
git push heroku master
```
