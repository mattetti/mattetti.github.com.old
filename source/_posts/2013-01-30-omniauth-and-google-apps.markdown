---
layout: post
title: "OmniAuth and Google Apps"
date: 2013-01-30 19:11
comments: true
categories: 
- Ruby
- authentication
---

Today I struggled to get [OmniAuth](https://github.com/intridea/omniauth) and [Google apps](https://developers.google.com/accounts/docs/OpenID) to work properly together.
I just wanted to add authentication to my application and restrict access to onl my Google Apps domain users.
I was hoping it would be straight forward since I could use Google's OpenID service.

Turns out it wasn't that hard, but the lack of documentation made me
lost a couple hours.
I therefore updated [OmniAuth's wiki](https://github.com/intridea/omniauth/wiki) and wrote this quick post so hopefully you won't waste time looking for simple details.

## Requirements

You actually only need to add 2 gems to your Gemfile:

```ruby
gem 'omniauth-openid'
gem 'ruby-openid-apps-discovery'
```

Now, the second gem is the one I didn't know about. 
The gem is actually provided by [Google itself](https://github.com/google/ruby-openid-apps-discovery). It turns out, Google Apps use a custom discovery protocol.
They monkey patched the popular OpenID Ruby libraries so you can just drop in
their gem and their discovery system will magically work.

## Setup

You need to require 4 files to get everything setup properly:

```ruby
require 'omniauth-openid'
require 'openid'
require 'openid/store/filesystem'
require 'gapps_openid'
```

The first one is the omniauth extension for OpenID, the second one is
the main Ruby OpenID library (needed so we can set our SSL cert).
The third one allows us to store temporary data on disk instead of
keeping it in memory (optional).
And finally, the last one is Google's magical gem to get their discovery
system working.

Because we are going to communicate via SSL, we want to make sure that
the OpenID library uses our certs to verify the SSL communications:

```ruby
OpenID.fetcher.ca_file = "/absolute/path/to/ssl_cacert.pem"
```
(Obviously, you need to change the path to your own cert)

We are almost with the setup, we just need two more things:

* make sure you are using a session.
* setup OmniAuth

I'm using Sinatra, so I'll load the `Rack::Session` middlware before I
set Omniauth:

```ruby
use Rack::Session::Cookie, :secret => 'supers3cr3t'
```

(Rails turns that option by default, so you don't need to worry about
it)

Then I can finally setup OmniAuth:

```ruby
use OmniAuth::Builder do
  provider :open_id,  :name => 'admin',
                      :identifier => 'https://www.google.com/accounts/o8/site-xrds?hd=aimonetti.net',
                      :store => OpenID::Store::Filesystem.new('/tmp')
end
```

There are two important things to notice. First, because I set the
provider's name to be 'admin', the magical paths provided by OmiAuth
will use that name (`/auth/admin`). Secondly, and more importantly, notice how I added
the name of my Google Apps domain at the end of the identifier:

```ruby
'https://www.google.com/accounts/o8/site-xrds?hd=' + your_domain_name
```

## Sinatra

In Sinatra, your just need to define the routes OmniAuth would use (same
goes for Rails, just use the router for that).

By default, omniauth now offers you a `/auth/admin` endpoint that will
push the user through Google Apps authentication.
Once the authentication is over, the user will be redirected to the
following endpoint:

```ruby
# Callback URL used when the authentication is done
post '/auth/admin/callback' do
  auth_details = request.env['omniauth.auth']
  session[:email] = auth_details.info['email']
  redirect '/auth/admin/welcome'
end
```

You can access the authentication details from `request.env['omniauth.auth']`
and redirect the user to another page, like the admin landing page for
instance.

```ruby
get '/auth/admin/welcome' do
  if session[:email]
    erb :welcome_boss
  else
    redirect '/auth/admin'
  end 
end
```

On the landing page, you need to verify that the user is logged in, in
this case, during the previous step, we added the email of the user to
her session. We can therefore verify the presence if that information to
confirm the authentication status. If the user is authenticated, then we'll render an ERB
template otherwise we'll redirect her back to the login page.

We should also provide an endpoint in case the authentication failed:

```ruby
get '/auth/failure' do
  params[:message]
  # do whatever you want here.
end
```

Note that by default, in dev mode, Omniauth won't redirect the user
there. To enable this behavior, use the following snippet (works with any rack app,
Rails, Sinatra or whatever):

```ruby
OmniAuth.config.on_failure = Proc.new { |env|
  OmniAuth::FailureEndpoint.new(env).redirect_to_failure
}
```

## Conclusion

Using Google Apps for authentication with OmniAuth is trivial as long
you know two things:

* the identifier url: `'https://www.google.com/accounts/o8/site-xrds?hd=' + your_domain_name`
* Google's discovery service gem

This blog post was written using `omniauth 1.1.1`, `omniauth-openid 1.0.1`, `rack-openid 1.3.1` and `ruby-openid-apps-discovery 1.2.0`. This might not apply to you if you come from the future :)

