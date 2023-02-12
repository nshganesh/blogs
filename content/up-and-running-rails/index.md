---
title: "Up and running Rails"
description: "Setting up rails environment ->"
date: "2023-02-12T08:55:10.876Z"
categories: []
published: false
---

Setting up rails environment ->

1.  install ruby and check ruby -v
2.  install rails and check rails -version
3.  Go to command line. and type — rails new appName. This creates a new rail app for you.
4.  Go to the created folder and type bin/rails server.
5.  If everything works fine, you should be able to see below screen on [http://localhost:3000](http://localhost:3000)

Rails up and running

Follow this guide if you get stuck somewhere [http://guides.rubyonrails.org/getting\_started.html](http://guides.rubyonrails.org/getting_started.html)

  

---

Creating controller and views for our application

Rails does most of our work. You just have to type some commands and that’s it.

1.  Let’s create controller, type: “rails g controller Resume index” , without quotes. Which creates a controller Resume and action called index.
2.  Most important files are controller — resume\_controller.rb and view — resume/index.html.
3.  Edit index.html with an html

`<h1>Hello, Rails!</h1>`

4\. Open config/routes.rb, this is our routing file. This is how we are telling rails how to connect incoming requests to controller and actions.

`Rails.application.routes.draw do`

`get _'resume/index'_`

`root _'resume#index'_`

`end`

5\. Launch the web server again. You will see hello rails message

---

Let’s start coding

`Rails.application.routes.draw do`

`get _'resume/index'_`

`resources **:contents**`

`root _'resume#index'_`

`end`

1.  Add the above line, that will create restful resources for contents.
2.  We will see what that means. Before that let’s create controller.
3.  open terminal and run

rails generate controller Contents

4. 

  

---

Using PDF gem prawn

Add this to your Gemfile

```
gem 'prawn', '~>2.2.0'
```

And in contents\_controller

```
def show
    respond_to do |format|
      // some other formats like: format.html { render :show }

      format.pdf do
        pdf = Prawn::Document.new
        pdf.text "Hellow World!"
        send_data pdf.render,
          filename: "export.pdf",
          type: 'application/pdf',
          disposition: 'inline'
      end
    end
  end
```

Now you can see the pdf here

[http://0.0.0.0:3000/contents/show.pdf](http://0.0.0.0:3000/contents/show.pdf)
