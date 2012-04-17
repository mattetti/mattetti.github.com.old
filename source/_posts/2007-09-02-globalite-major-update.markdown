---
date: '2007-09-02 22:21:00'
layout: post
legacy_url: http://railsontherun.com/2007/09/02/globalite-major-update/
slug: globalite-major-update
source: railsontherun.com
status: publish
title: Globalite Major update
wordpress_id: '88'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- activeRecord
- chinese
- globalite
- i18n
- l10n
- localization
- rails
- simplified chinese
- traditional chinese
- translation
---

## _I'm glad to announce a major update of Globalite._





First off I'd like to thank all the translators who helped with this release. 







  * Globalite now support its **first Asian language: Chinese!**
Ivan Chang did an awesome job creating a localization file in Chinese for Taiwan, Hong Kong and Main Land China. I'm really glad thinking that Globalite will make the Rails experience much nicer for a lot of Chinese people.



  * Ivan also pushed me to add a new feature that people had asked about: a **better ActiveRecord error message support**.






You know how Rails has a nice way of displaying your Model errors:





![AR_error](http://farm2.static.flickr.com/1052/1306879608_e60431d214_o.png)





Well, now that's automatically translated in your locale. (as long as the new localization files are up to date. Feel free to contact me if you want to improve the locale file in your own language)







  * I also added support for pluralization directly in the translation file. (pluralization doesn't always make sense in some languages) I'm planning on adding a better Inflector support later on.





for now, in your translation file simply use:




    
<code>horse_count: we have pluralize curly_brace curly_brace count curly_brace, horse curly_brace in the
ranch</code>




In your view use the localization with arguments to pass the count:




    
<code>
<%= :horse_count.l_with_args({:count => @horse.count}) %>
</code>




See [the wiki](http://code.google.com/p/globalite/wiki/PluralizationSupport) for more information about pluralization.







  * Finally the [demo app](http://globalite.googlecode.com/svn/sample/ui/) has been updated with an example of how to grab the acceptable locale from the header. (thanks to Emmanuel Bouton)


