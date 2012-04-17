---
date: '2012-04-04 07:29:16'
layout: post
slug: building-and-implementing-a-single-sign-on-solution
status: publish
title: Building and implementing a Single Sign-On solution
wordpress_id: '1301'
categories:
- blog-post
- Software Design
- Tutorial
tags:
- architecture
- crypto
- SSO
---

Most modern web applications start as a monolithic code base and, as complexity increases, the once small app gets split apart into many "modules". In other cases, engineers opt for a [SOA](http://en.wikipedia.org/wiki/Service-oriented_architecture) design approach from the beginning. One way or another, we start running multiple separate applications that need to interact seamlessly. My goal will be to describe some of the high-level challenges and solutions found in implementing a Single-Sign-On service.


## Authentication vs Authorization


I wish these two words didn't share the same root because it surely confuses a lot of people. My most frequently-discussed example is [OAuth](http://en.wikipedia.org/wiki/OAuth). Every time I start talking about implementing a centralized/unified authentication system, someone jumps in and suggests that we use [OAuth](http://en.wikipedia.org/wiki/OAuth). The challenge is that [OAuth](http://en.wikipedia.org/wiki/OAuth) is an authorization system, not an authentication system.

It's tricky, because you might actually be "authenticating" yourself to website X using OAuth. What you are really doing is allowing website X to use your information stored by the OAuth provider. It is true that OAuth offers a pseudo-authentication approach via its provider but that is not the main goal of [OAuth](http://en.wikipedia.org/wiki/OAuth): the Auth in OAuth stands for Authorization, not Authentication.

Here is how we could briefly describe each role:



	
  * **Authentication**: recognizes who you are.

	
  * **Authorization**: know what you are allowed to do, or what you allow others to do.


If you are feel stuck in your design and something seems wrong, ask yourself if you might be confused by the 2 auth words. This article will only focus on **authentication**.


## A Common Scenario




[![SSO diagram with 3 top applications connecting to an authorization service.](http://merbist.com/wp-content/uploads/2012/04/SSO-simplescenario.png)](http://merbist.com/wp-content/uploads/2012/04/SSO-simplescenario.png)


This is probably the most common structure, though I made it slightly more complex by drawing the three main apps in different programming languages. We have three web applications running on different subdomains and sharing account data via a centralized authentication service.

**Goals:**



	
  * Keep authentication and basic account data isolated.

	
  * Allow users to stay logged in while browsing different apps.


Implementing such a system should be easy. That said, if you migrate an existing app to an architecture like that, you will spend 80% of your time decoupling your legacy code from authentication and wondering what data should be centralized and what should be distributed. Unfortunately, I can't tell you what to do there since this is very domain specific. Instead, let's see how to do the "easy part."


## Centralizing and Isolating Shared Account Data


At this point, you more than likely have each of your apps talk directly to shared database tables that contain user account data. The first step is to migrate away from doing that. We need a single interface that is the only entry point to create or update shared account data. Some of the data we have in the database might be app specific and therefore should stay within each app, anything that is shared across apps should be moved behind the new interface.

Often your centralized authentication system will store the following information:



	
  * ID

	
  * first name

	
  * last name

	
  * login/nickname

	
  * email

	
  * hashed password

	
  * salt

	
  * creation timestamp

	
  * update timestamp

	
  * account state (verified, disabled ...)


Do not duplicate this data in each app, instead have each app rely on the account ID to query data that is specific to a given account in the app. Technically that means that instead of using SQL joins, you will query your database using the ID as part of the condition.

My suggestion is to do things slowly but surely. Migrate your database schema piece by piece assuring that everything works fine. Once the other pieces will be in place, you can migrate one code API a time until your entire code base is moved over. You might want to change your DB credentials to only have read access, then no access at all.


## Login workflow


Each of our apps already has a way for users to login. We don't want to change the user experience, instead we want to make a transparent modification so the authentication check is done in a centralized way instead of a local way. To do that, the easiest way is to keep your current login forms but instead of POSTing them to your local apps, we'll POST them to a centralized authentication API. (SSL is strongly recommended)


[![diagram showing the login workflow](http://merbist.com/wp-content/uploads/2012/04/SSO-login.png)](http://merbist.com/wp-content/uploads/2012/04/SSO-login.png)


As shown above, the login form now submits to an endpoint in the authentication application. The form will more than likely include a login or email and a clear text password as well as a hidden callback/redirect url so that the authentication API can redirect the user's browser to the original app. For security reasons, you might want to white list the domains you allow your authentication app to redirect to.

Internally, the Authentication app will validate the identifier (email or login) using a hashed version of the clear password against the matching record in the account data. If the verification is successful, a token will be generated containing some user data (for instance: id, first name, last name, email, created date, authentication timestamp). If the verification failed, the token isn't generated. Finally the user's browser is redirected to the callback/redirect URL provided in the request with the token being passed.

You might want to safely encrypt the data in a way that allows the clients to verify and trust that the token comes from a trusted source. A great solution for that would be to use [RSA encryption](http://en.wikipedia.org/wiki/RSA_(algorithm)) with the public key available in all your client apps but the private key only available on the auth server(s). Other strong encryption solutions would also work. For instance, another appropriate approach would be to add a signature to the params sent back. This way the clients could check the authenticity of the params. [HMAC](http://en.wikipedia.org/wiki/HMAC) or [DSA](http://en.wikipedia.org/wiki/Digital_Signature_Algorithm) signature are great for that but in some cases, you don't want people to see the content of the data you send back. That's especially true if you are sending back a 'mobile' token for instance. But that's a different story. What's important to consider is that we need a way to ensure that the data sent back to the client can't be tampered with. You might also make sure you prevent replay attacks.

On the other side, the application receives a GET request with a token param. If the token is empty or can't be decrypted, authentication failed. At that point, we need to show the user the login page again and let him/her try again. If on the other hand, the token can be decrypted, the content should be saved in the session so future requests can reuse the data.

We described the authentication workflow, but if a user logins in application X, (s)he won't be logged-in in application Y or Z. The trick here, is to set a top level domain cookie that can be seen by all applications running on subdomains. Certainly, this solution only works for apps being on the same domain, but we'll see later how to handle apps on different domains.


[![](http://merbist.com/wp-content/uploads/2012/04/SSO-login-cookie.png)](http://merbist.com/wp-content/uploads/2012/04/SSO-login-cookie.png)


The cookie doesn't need to contain a lot of data, its value can contain the account id, a timestamp (to know when authentication happened and a trusted signature) and a signature. The signature is critical here since this cookie will allow users to be automatically logged in other sites. I'd recommend the  [HMAC](http://en.wikipedia.org/wiki/HMAC) or [DSA](http://en.wikipedia.org/wiki/Digital_Signature_Algorithm) encryptions to generate the signature. The DSA encryption, very much like the RSA encryption is an asymmetrical encryption relying on a public/private key. This approach offers more security than having something based a shared secret like HMAC does. But that's really up to you.

Finally, we need to set a filter in your application. This auto-login filter will check the presence of an auth cookie on the top level domain and the absence of local session. If that's the case, a session is automatically created using the user id from the cookie value after the cookie integrity is verified. We could also share the session between all our apps, but in most cases, the data stored by each app is very specific and it's safer/cleaner to keep the sessions isolated. The integration with an app running on a different service will also be easier if the sessions are isolated.




## Registration


For registration, as for login, we can take one of two approaches: point the user's browser to the auth API or make S2S (server to server) calls from within our apps to the Authentication app. POSTing a form directly to the API is a great way to reduce duplicated logic and traffic on each client app so I'll demonstrate this approach.


[![](http://merbist.com/wp-content/uploads/2012/04/CopyofSSO-register.png)](http://merbist.com/wp-content/uploads/2012/04/CopyofSSO-register.png)


As you can see, the approach is the same we used to login. The difference is that instead of returning a token, we just return some params (id, email and potential errors). The redirect/callback url will also obviously be different than for login. You could decide to encrypt the data you send back, but in this scenario, what I would do is set an auth cookie at the .domain.com level when the account is created so the "client" application can auto-login the user. The information sent back in the redirect is used to re-display the register form with the error information and the email entered by the user.

At this point, our implementation is almost complete. We can create an account and login using the defined credentials. Users can switch from one app to another without having to re login because we are using a shared signed cookie that can only be created by the authentication app and can be verified by all "client" apps. Our code is simple, safe and efficient.


## Updating or deleting an account


The next thing we will need is to update or delete an account. In this case, this is something that needs to be done between a "client" app and the authentication/accounts app. We'll make S2S (server to server) calls. To ensure the security of our apps and to offer a nice way to log requests, API tokens/keys will be used by each client to communicate with the authentication/accounts app. The API key can be passed using a [X-header](http://en.wikipedia.org/wiki/List_of_HTTP_header_fields) so this concern stays out of the request params and our code can process separately the authentication via X-header and the actual service implementation. S2S services should have a filter verifying and logging the API requests based on the key sent with the request. The rest is straight forward.


## Using different domains


Until now, we assumed all our apps were on the same top domain. In reality, you will often find yourself with apps on different domains. This means that you can't use the shared signed cookie approach anymore. However, there is a simple trick that will allow you to avoid requiring your users to re-login as they switch apps.


[![](http://merbist.com/wp-content/uploads/2012/04/SSO-differentdomains-1.png)](http://merbist.com/wp-content/uploads/2012/04/SSO-differentdomains-1.png)




The trick consists, when a local session isn't present, of using an iframe in the application using the different domain. The iframe loads a page from the authentication/accounts app which verifies that a valid cookie was set on the main top domain. If that is the case, we can tell the application that the user is already globally logged in and we can tell the iframe host to redirect to an application end point passing an auth token the same way we did during the authentication. The app would then create a session and redirect the user back to where (s)he started. The next requests will see the local session and this process will be ignored.

If the authentication application doesn't find a signed cookie, the iframe can display a login form or redirect the iframe host to a login form depending on the required behavior.

Something to keep in mind when using multiple apps and domains is that you need to keep the shared cookies/sessions in sync, meaning that if you log out from an app, you need to also delete the auth cookie to ensure that users are globally logged out. (It also means that you might always want to use an iframe to check the login status and auto-logoff users).




## Mobile clients


Another part of implementing a SSO solution is to handle mobile clients. Mobile clients need to be able to register/login and update accounts. However, unlike S2S service clients, mobile clients should only allow calls to modify data on the behalf of a given user. To do that, I recommend providing opaque mobile tokens during the login process. This token can then be sent with each request in a X-header so the service can authenticate the user making the request. Again, SSL is strongly recommended.

In this approach, we don't use a cookie and we actually don't need a SSO solution, but an unified authentication system.




## Writing web services


Our Authentication/Accounts application turns out to be a pure web API app.

We also have 3 sets of APIs:



	
  * Public APIs: can be accessed from anywhere, no authentication required

	
  * S2S APIs: authenticated via API keys and only available to trusted clients

	
  * Mobile APIs: authenticated via a mobile token and limited in scope.


We don't need dynamic HTML views, just simple web service related code. While this is a little bit off topic, I'd like to take a minute to show you how I personally like writing web service applications.

Something that I care a lot about when I implement web APIs is to validate incoming params. This is an opinionated approach that I picked up while at Sony and that I think should be used every time you implement a web API. As a matter of fact, I wrote a Ruby [DSL library (Weasel Diesel)](https://github.com/mattetti/Weasel-Diesel) allowing you [describe a given service](https://github.com/mattetti/sinatra-web-api-example/blob/master/api/hello_world.rb), its [incoming params](https://github.com/mattetti/sinatra-web-api-example/blob/master/api/hello_world.rb#L7), and the [expected output](https://github.com/mattetti/sinatra-web-api-example/blob/master/api/hello_world.rb#L10-15). This DSL is hooked into a web backend so you can implement services using a web engine such as [Sinatra](http://www.sinatrarb.com/) or maybe Rails3. Based on the DSL usage, incoming parameters are be verified before being processed. The other advantage is that you can generate documentation based on the API description as well as automated tests.

You might be familiar with [Grape](https://github.com/intridea/grape), another DSL for web services. Besides the obvious style difference [Weasel Diesel ](https://github.com/mattetti/Weasel-Diesel)offers the following advantages:



	
  * input validation/sanitization

	
  * service isolation

	
  * generated documentation

	
  * contract based design




Here is a hello world webservice being implemented using Weasel Diesel and Sinatra:


[gist]https://gist.github.com/2300131[/gist]

Basis test validating the contract defined in the DSL and the actual output when the service is called:

[gist]https://gist.github.com/2300440[/gist]

Generated documentation:


![](https://img.skitch.com/20120404-t1j93b73tef5pmd5idfqqa61td.jpg)


If the DSL and its features seem appealing to you and you are interested in digging more into it, the easiest way is to fork [this demo repo](https://github.com/mattetti/sinatra-web-api-example/) and start writing your own services.

The DSL has been used in production for more than a year, but there certainly are tweaks and small changes that can make the user experience even better. Feel free to fork the [DSL repo](https://github.com/mattetti/Weasel-Diesel) and send me Pull Requests.
