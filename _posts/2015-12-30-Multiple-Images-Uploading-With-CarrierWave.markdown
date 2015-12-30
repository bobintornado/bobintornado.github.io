---
layout: post
comments: true
title:  "Multiple Images Uploading With CarrierWave"
date:   2015-12-30 21:24:36
categories: Rails
---

TL;DR. Jump to [key points](http://localhost:4000/rails/2015/12/31/Multiple-Images-Uploading-With-CarrierWave.html#keypoints) section.

In this blog post I am going to demonstrate how to implement multiple images uploading related features by using CarrierWave, and I am going to illustrate the technique in the context of a sample gallery application.

To be specific about the features, we are going to implement the following ones:

1. Create a new gallery by uploading multiple images
3. Upload more images into existing gallery
4. Delete specific image from existing gallery (one at a time)

And roughly I am going to build this sample application in the following way:

1. Use Rails 4.2.5 as the overall framework
2. Use CarrierWave as the gem for handling multiple files uploading. And I am going to use the master branch, since according to the [documentation](https://github.com/carrierwaveuploader/carrierwave), this feature is only enabled on the master branch
3. Use PostgreSQL as the database, and particularly exploit the convenient array type supported by PostgreSQL for storing a sequence of \"*images*\".
4. Use Amazon S3 for storing the actual images uploaded.
5. Use Slim as the template engine and some HTML5 features, particular the \<input\> multiple attribute (eg. `<input type="file" name="imgs" multiple>`)

And now let's build this application step by step.

1. Generate a new Rails application, install some gems and do some configuration.
2. Configured the CarrierWave and create the uploader.
3. Create our models & its migration
4. Create the routes and controllers
5. Prepare relevant views
6. Smooth up the user interaction with some CoffeeScript
7. Re-factoring

## Step 1 Setup the Rails Application

First let's run `rails new sample-gallery-app` to generate our new gallery application.

And inside our `Gemfile`, I specify the Ruby version to `2.2.4` by adding `ruby "2.2.4"`.

I remove gem `sqlite3`, and add following gems 

* gem "pg", "0.18.1"
* gem "slim-rails", "~> 3.0.1"
* gem "bootstrap-sass", "~> 3.3.6"
* gem "carrierwave", :github => "carrierwaveuploader/carrierwave"
* gem "fog-aws"

After this, let's run `bundle` to get everything installed.

After installing the gems, let's configure `config/database.yml` to use `postgresql` instead of `sqlite3`.


## Step 2 Configure CarrierWave

Let's create the initialization file `/config/initializers/carrier_wave.rb` and copy & paste in the example configuration from official documentation.

{% highlight ruby %}

CarrierWave.configure do |config|
  config.fog_provider = 'fog/aws'                        # required
  config.fog_credentials = {
    provider:              'AWS',                        # required
    aws_access_key_id:     'xxx',                        # required
    aws_secret_access_key: 'yyy',                        # required
    region:                'eu-west-1',                  # optional, defaults to 'us-east-1'
    host:                  's3.example.com',             # optional, defaults to nil
    endpoint:              'https://s3.example.com:8080' # optional, defaults to nil
  }
  config.fog_directory  = 'name_of_directory'                          # required
  config.fog_public     = false                                        # optional, defaults to true
  config.fog_attributes = { 'Cache-Control' => "max-age=#{365.day.to_i}" } # optional, defaults to {}
end

{% endhighlight %}

You need to replace sample credentials with yours.

And then Let's generate our image uploader by calling `rails generate uploader Image`, this should generate a new file at path `app/uploaders/image_uploader.rb`.

For this newly generated uploader, let's change the `storage :file` to `storage :fog`. 

## Step 3 Create Models

Now we have the our class `ImageUploader`, we could go ahead and generate the model we need, namely the `gallery` model.

Let's Run `rails g model gallery`, and add `mount_uploaders :images, ImageUploader` into the class, so that `/app/models/gallery.rb` looks like 

{% highlight ruby %}

class Gallery < ActiveRecord::Base
  mount_uploaders :images, ImageUploader
end

{% endhighlight %}

`mount_uploaders :images, ImageUploader`

Then lets create the migration for this model by calling:

`rails g migration AddDetailsToGallery`

And in the generated migration file, let's write 

{% highlight ruby %}

class AddDetailsToGallery < ActiveRecord::Migration
  def change
    add_column :galleries, :name, :string
    add_column :galleries, :images, :string, array: true, default: []
  end
end

{% endhighlight %}

This is how we add a array column to our Gallery model to hold the images.

## Step 4 Create Routes and Controllers

Let's create our routes, and nest images resources under galleries resources, so that our routes file should look like something below

{% highlight ruby %}

Rails.application.routes.draw do
  resources :galleries, do
    resources :images, :only => [:create, :destroy]
end

{% endhighlight %}

We setup routes this way so that we could easily modify each image individually.

And now let's generate our controllers by calling 

`rails generate controller Galleries` and `rails generate controller Images`

Let's fill up the GalleriesController with standard CRUD actions.
