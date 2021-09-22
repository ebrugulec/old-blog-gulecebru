---
layout: post
title: Rails-wicked_pdf gem export data on heroku
date:   2018-12-02 19:07:19
modified:  2018-12-02
comments: true
---

If we want to export our records as pdf in rails, we can use wicked_pdf or another gems. I used wicked_pdf. Because easy to configuration and set up. wicked_pdf gem is using [wkhtmltopdf](http://wkhtmltopdf.org/) to serve a PDF file to a user from HTML. When you set up configuration [this](http://github.com/mileszs/wicked_pdf) page everything is okay. Your records will be successfully exported as pdf. But if you want to deploy your app heroku or another system, you should make a few configuration. I will tell heroku configuration this post.

#### Firs Step — Download wkhtmltopdf
You need to [wkhtmltopdf-amd64 ](http://github.com/h4cc/wkhtmltopdf-amd64) project. Download this project and create a folder in your root directory. Call the folder as bin. And copy wkhtmltopdf-amd64 project inside bin folder.

#### Second Step — Set up the Configuration

config/wicked_pdf.rb
```ruby
if Rails.env.staging? || Rails.env.production?
  exe_path = Rails.root.join('bin', 'wkhtmltopdf-amd64').to_s
else
  WickedPdf.config = {
     exe_path:  'C:\wkhtmltopdf\bin\wkhtmltopdf.exe'
  }
end
```

#### Last Step

In production mode, you need to do the following configurations in order not to have any problems in your asset files.

config/production.rb

```
config.serve_static_assets = true
```

initializers/assets.rb
```
Rails.application.config.assets.precompile += %w( nested_form.js application.css.scss application.js )
```

Resources:

[set up wicked gem](http://gist.github.com/wrburgess/3778949)