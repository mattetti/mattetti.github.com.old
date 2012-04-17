---
date: '2007-07-03 04:00:00'
layout: post
legacy_url: http://railsontherun.com/2007/07/03/globalite-usage-example/
slug: globalite-usage-example
source: railsontherun.com
status: publish
title: Globalite - usage example
wordpress_id: '45'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- date_select
- example
- globalite
- i18n
- localization
---

If you haven't check out the [Globalite sample app](http://globalite.googlecode.com/svn/sample/ui/) yet, here are some screenshots.





Here is the French yml file used for the UI translation/localization:





![create a yml file with the translation](http://myskitch.com/matt_a/fr.yml__ui-20070702-200052.jpg)





Here is the code to display a select box so the user can choose the publication date:





![write some code to let the user choose a published date ](http://myskitch.com/matt_a/date_select-code-20070702-195558.jpg)





So you know,




    
    <code><%= :published_date.l %>
    </code>





is the same as 




    
    <code><%= :published_date.localize %>
    </code>





Globalite will try to load the translation of the :published_date key using the locale you set. (if you didn't set a locale, it will try in English. If there was no English translation available, a warning message would be displayed instead of the translation. We could have also done the following:




    
    <code><%= :published_date.l('Published Date') %>
    </code>





If you pass a string to the localization function, Globalite will display it in the case where it doesn't find a translation.





Here is the default view in English:





![let's look at the view in English (default locale)](http://myskitch.com/matt_a/date_select_en-20070702-192452.jpg)





Change the locale to French  ( Locale.code = 'fr-*'  or use the select box in the demo app).
Let's display the same page:





![same date select box in French](http://myskitch.com/matt_a/date_select_fr-20070702-192257.jpg)





Note that in French, the date is different, on top of having translated month names, the order is different. American English format is year/month/day while the French format is day/month/year.





You get all of that for free and even more.





Just for the fun, here is another example of localization: currency. you might know it, but each country, on top of having a different currency display numbers differently.





In the England, it would look like that: 





![uk](http://myskitch.com/matt_a/nbr_to_currency-uk-20070702-203822.jpg)





Note that the currency is before the amount and that there's a separator for the thousands and a delimiter for the pence.





In France, it would look like that:





![fr](http://myskitch.com/matt_a/nbr_to_currency-fr-20070702-202242.jpg)





The currency is after the amount and the French separator is actually the British delimiter and vice versa!





Spain also uses the Euro as currency but they don't display the same amount in the same way:





![es](http://myskitch.com/matt_a/nbr_to_currency-es-20070702-203736.jpg)





The code to display the amount for each locale is the following:




    
    <code><%= number_to_currency(9999.99) %>
    </code>





Pretty cool and simple, isn't it? Check out the [sample app](http://globalite.googlecode.com/svn/sample/ui/) for more advance used.





_Enjoy_





p.s: thanks [josh knowles](http://joshknowles.com) for the [skitch](http://plasq.com/skitch/) invite, it's awesome!
