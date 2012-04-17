---
date: '2007-06-27 21:53:00'
layout: post
legacy_url: http://railsontherun.com/2007/06/27/globalite-update/
slug: globalite-update
source: railsontherun.com
status: publish
title: Globalite update
wordpress_id: '43'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- globalite
- i18n
- internationalization
- l10n
- locale
- localization
- rails
- translation
---

Today I updated Globalite, refactoring the code and fixing some issues.





I added a bunch of aliases since I was tired of typing:




    
    <code>Globalite.current_language = :fr
    </code>





I shortened it to 




    
    <code>Globalite.language = :fr
    </code>





But the old method is still available so it won’t break your code ;)





The major change with this update is a better support for locales. Let’s say that in our application we setup the language to :fr (French) but we didn’t specify a country. Remember that Globalite lets you have specific translations/localizations per country so you can display dates, prices properly.  





Previously, if my locale was set to :fr-* then Globalite would try to load the fr.yml files and not fr-FR.yml.





If you decided to use a fr.yml file for your UI localization, you might have noticed that all the Rails core messages weren’t translated(now you know why). This update deals with this issue and if a translation isn’t found in your locale, it will look for translations in your language but from a different country and pick a random one.





I’m expecting Rails core translations in Japanese, Korean and Chinese. Feel free to email me your own file and let me know on what project you used Globalite.
