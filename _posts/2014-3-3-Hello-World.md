---
layout: post
title: Sending Email from Rails < 2.2 app via SMTP
---

With a rails app that is using rails < 2.2 and ruby 1.8.7, when we config to sending email, we get an error: "Net::SMTPAuthenticationError: 530 Must issue a STARTTLS command first". It's related with SMTP TLS issue. We can resolve this issue by using [smtp-tls](https://github.com/ambethia/smtp-tls). For manually, you can do step by step: 

- Step 1: Copy [smtp-tls.rb](https://github.com/ambethia/smtp-tls/blob/master/lib/smtp-tls.rb) to lib folder in your app.
- Step 2: Create a file in config/initializers folder in your app with the content like: 
    
    ```
    
    require 'smtp-tls'

    ActionMailer::Base.smtp_settings = {
      :address              => 'email-smtp.us-east-1.amazonaws.com',
    	:port                 => 587,
    	:domain               => "yourdomain.com",
      :user_name            => "your_username",
      :password             => "your_password",
      :authentication       => :plain,
      :enable_starttls_auto => true
    }
    ```
    
Restart your app and test it.

If you’re running Rails >= 2.2.1 [RC2] and Ruby 1.8.7, you don’t need the script (smtp-tls) above. Ruby 1.8.7 supports SMTP TLS and Rails 2.2.1 ships with an option to enable it if you’re running Ruby 1.8.7.

All You need to do is:
    ```
    ActionMailer::Base.smtp_settings = {
        :enable_starttls_auto => true
    }
    ```

Thanks,
