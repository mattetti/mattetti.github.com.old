---
date: '2007-05-15 15:58:00'
layout: post
legacy_url: http://railsontherun.com/2007/05/15/globalite-i18n-i10n-solution/
slug: globalite-i18n-i10n-solution
source: railsontherun.com
status: publish
title: Globalite or another i18n/l10n solution
wordpress_id: '20'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- globalite
- i18n
- internationalization
- l10n
- localization
- plugin
- rails
- translation
---

I've been recently slightly annoyed by the lack of simple yet powerful internationalization/localization plugin for Rails.
It seems like the only real solution out there is [Globalize](http://www.globalize-rails.org)





However, it's a heavy, bloated, hard to setup plugin. I might seem to complain, but I'm actually glad that someone came up with a decent solution. Globalize is quite powerful but I would like to see something simpler, lighter and easier to use.





While I was looking for alternate solutions, [Err](http://errtheblog.com/) released [Gibberish](http://errtheblog.com/post/4396) a simple and nice localization plugin.





Looking at Chris' code, I found a clean and nice solution but unfortunately too simple. The major problem I had with Giberrish was the lack of support for locales. That means that one cannot create an application supporting British English and American English. I lived in  both America and the UK and believe me, I can never spell some words properly. How should I spell the following words: colours or colors, behaviour or behavior, aluminium or aluminum? On top of that, your UI might be slightly different based on the region your visitor is coming from. Localization is more than translation.





Anyway, I decided to come up with my own solution, a breed of the best features from my favorite i18n/l10n projects. This hybrid solution should, hopefully be useful for people not needing all the resources of Globalize. I called my new project _Globa_lite__





## What's cool with Globalite?







  * Simple UI localization





    * support different locales


    * yml file based


    * simple syntax


    * no setup needed


    * enough for most projects needing localization


    * pass variables to the localization and let the translator deal with it




  * Simple Rails Localization





    * support different locales


    * nothing to do, rails comes in your own language (including currency conversion and other helpers)


    * yml file based localization for easy update


    * support for Rails 1.2x


    * simple to maintain


    * simple to add/edit a new locale




  * Simple content i18n/l10n (coming soon)






## UI localization:







  * Create a lang/ui folder at the root of the folder.


  * create 2 localization files: fr.yml and en-US.yml


  * add localization keys in the files:





fr.yml:




    
    <code>hello: bonjour
    
    welcome: bienvenue
    </code>





en-US.yml




    
    <code>hello: howdy
    
    welcome: welcome
    </code>







  * Declare the current locale or language 




    
    <code>Globalite.current_language :fr
    </code>



  * In your view (or anywhere else) localize a key 




    
    <code>:hello.l
    </code>






or




    
    <code>:hello.localize
    </code>





you can also do 




    
    <code>:unknown_key.l("this is the default text to display if the localization isn't found")
    </code>





Also you can pass one or many variables to the localization(check the documentation)





## Rails Localization





Rails is automatically localized for you (meaning most helpers, including the currency converter are localized for the locale you declared).
You really easily add a new supported rails language/settings by adding a yml file or editing an existing one. (for now, I only create an English and a French localization file, but I'll add plenty more very soon)





Globalite also saves the locale in the session allowing many users to see a site at the same time in different languages :)





I believe Globalite is/will be a good solution for most of my international projects. It still lacks the content i18n and l10n as well as support for pluralization but it should be added soon.





Give it a try and let me know what you think:





[RubyForge page](http://rubyforge.org/projects/globalite)





svn repository: svn checkout svn://rubyforge.org/var/svn/globalite/trunk globalite





[view repository content](http://viewvc.rubyforge.mmmultiworks.com/cgi/viewvc.cgi/?root=globalite)





ps: The project is still in beta and will change during the next few weeks, feel free to submit patches, ideas, suggestions...
