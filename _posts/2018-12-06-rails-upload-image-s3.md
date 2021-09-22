---
layout: post
title: Rails-Uploading Files via Amazon S3 And Paperclip
date:   2018-12-06 19:07:19
modified:  2018-12-06
comments: true
img: rails-s3.png
---

#### Prerequisites

1. [AWS S3 Account](http://devcenter.heroku.com/articles/s3#s3-setup) for storing images in the cloud.
2. [ImageMagick](http://www.imagemagick.org/script/index.php) for resizing images.

Ruby Version: 2.4.4
Rails Version: Rails 5.2.1

You have to create bucket inside the s3 services in aws. And you must create user and IAM user for this bucket. Don’t forget to set permission this bucket. Copy your bucket AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY credidentals.

#### Easy Way for Environment Variables

Every applications require a configuration settings such as email account credentials or API keys for external services(security purpose). You can pass local configuration settings to an application using environment variables.

We will create a simple file that contains key/value pairs for each environment variable using the standard YAML file format.

##### Create a file config/local_env.yml:

‘# makes ‘AWS_ACCESS_KEY_ID’ available as ENV[“AWS_ACCESS_KEY_ID”]
```
AWS_ACCESS_KEY_ID: 'xxXXXXXxxxx'
```

##### Setting in .gitignore

If you have created a git repository for your application, your application root directory should contain a file named .gitignore.
add below line in your .gitignore file.

**/config/local_env.yml**

After setting env variables, we have to set local_env.yml file into config/application.rb file.
add the below code:

```ruby
config.before_configuration do
  env_file = File.join(Rails.root, 'config', 'local_env.yml')
  YAML.load(File.open(env_file)).each do |key, value|
    ENV[key.to_s] = value
  end if File.exists?(env_file)
end
```

The above code sets environment variables from local_env.yml file. Now you can set all enviroment variables inside this file.

```ruby
AWS_ACCESS_KEY_ID: "XXXxxxXXX"
AWS_SECRET_ACCESS_KEY: "XXXxxxXXX"
AWS_REGION: "us-east-2"
S3_BUCKET_NAME: "picture-animals"
```

Add your Gemfile paperclip and aws-sdk gem.

```ruby
gem ‘paperclip’, git: ‘http://github.com/thoughtbot/paperclip'
gem ‘aws-sdk’, ‘< 2.0’
gem ‘aws-sdk-s3’
```

Note: If you see any error, you can change paperclip this specific version.

```
http://github.com/thoughtbot/paperclip', :ref => '523bd46c768226893f23889079a7aa9c73b57d68'
```

We’ll also need to specify the AWS configuration variables for the model.

```ruby
has_attached_file :image,
    storage: :s3,
    s3_credentials: {
        access_key_id: ENV.fetch("AWS_ACCESS_KEY_ID"),
        secret_access_key: ENV.fetch('AWS_SECRET_ACCESS_KEY'),
        s3_region: ENV.fetch('AWS_REGION'),
        bucket: ENV.fetch('S3_BUCKET_NAME')
    },
    path: ":attachment/:id/:style/:filename",
    url: "http://s3.us-east-2.amazonaws.com",
    styles: {
        medium: ["250x250#"],
        small: ["200x200#"]
    },
    default_url: 'missing.png'
```

If you are seeing the following error: “The bucket you are attempting to access must be addressed using the specified endpoint. Please send all future requests to this endpoint.” Try setting the specified endpoint with the **s3_host_name** config var.

```ruby
s3_host_name: "s3-#{ENV.fetch('AWS_REGION')}.amazonaws.com"
```

Make sure the form has **multipart: true** added to it.

<%= nested_form_for @animal, html: { multipart: true } do |f| %>

#### Image display

Files that have been uploaded with Paperclip are stored in S3. However, metadata such as the file’s name, location on S3, and last updated timestamp are all stored in the model’s table in the database.

Access the file’s url through the url method on the model’s file attribute.

```ruby
animal.image.url #=>"//s3-us-east-2.amazonaws.com/picture..."
```

And you can use image tag for display image.

```ruby
<%= image_tag @animal.image.url(:small) %>
```

#### Resources:

[Paperclip Doc](http://www.rubydoc.info/github/thoughtbot/paperclip/Paperclip/Storage/S3)

[Ruby Thursday](http://www.youtube.com/watch?v=Tc1dXKUlI_A)

[Image is not showing up papaerclip s3](http://stackoverflow.com/questions/36410940/rails4-2-images-not-showing-up-using-aws-s3-paperclip-gem)

[NameError Paperclip Storage S3 AWS](http://stackoverflow.com/questions/28374401/nameerror-uninitialized-constant-paperclipstorages3aws)




