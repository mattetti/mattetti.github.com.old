---
date: '2008-01-04 03:46:00'
layout: post
legacy_url: http://railsontherun.com/2008/01/04/rspec-on-rails-matchers-plugin/
slug: rspec-on-rails-matchers-plugin
source: railsontherun.com
status: publish
title: RSpec on Rails Matchers plugin
wordpress_id: '454'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- matchers
- RSpec
- test
- testing
---

[RSpec](http://rspec.info) is an awesome testing framework. On top of being the first Ruby [BDD](http://en.wikipedia.org/wiki/Behavior_driven_development) framework the core team is doing a great job in enhancing our testing experience and therefore the quality of our code.





This time, I don't want to introduce to the [latest changes](http://rspec.info/changes.html) but instead showing you what [Josh Knowles](http://joshknowles.com), [Bryan Helmkamp](http://www.brynary.com/) and myself came up with.





[RSpec on Rails matchers plugin](http://code.google.com/p/rspec-on-rails-matchers/) + [TextMate Bundle](http://rspec-on-rails-matchers.googlecode.com/svn/textmate-bundle/RSpecOnRailsMatchers.tmbundle.zip)





Matchers are some sort of helpers that will help you cleaning up your tests. We simply came up with a collection of matchers that we think will make your like easier.





We divided the matchers in 3 categories:





## Associations





Verify that the association has been defined. (doesn't verify that the association works!)





_Usage examples:_






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>6<tt>
    </tt>7<tt>
    </tt>8<tt>
    </tt>


  
    
    <tt>
    </tt>    <span class="iv">@post</span>.should have_many(<span class="sy">:comments</span>)<tt>
    </tt>  <tt>
    </tt>    <span class="iv">@comment</span>.should belong_to(<span class="sy">:post</span>)<tt>
    </tt>  <tt>
    </tt>    <span class="iv">@user</span>.should have_one(<span class="sy">:social_security_number</span>)<tt>
    </tt>  <tt>
    </tt>    <span class="iv">@project</span>.should have_and_belong_to_many(<span class="sy">:categories</span>)<tt>
    </tt>






## Validations





Verify that a validation has been defined. (doesn't test the validation itself)






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>6<tt>
    </tt>7<tt>
    </tt>8<tt>
    </tt>9<tt>
    </tt><strong>10</strong><tt>
    </tt>


  
    
    <tt>
    </tt>    object.should validate_presence_of(<span class="sy">:attribute</span>)<tt>
    </tt><tt>
    </tt>    object.should validate_confirmation_of(<span class="sy">:attribute</span>)<tt>
    </tt><tt>
    </tt>    object.should validate_uniqueness_of(<span class="sy">:attribute</span>)<tt>
    </tt><tt>
    </tt>    object.should validate_length_of(<span class="sy">:attribute</span>, <span class="sy">:between</span> => <span class="i">5</span>..<span class="i">10</span>)<tt>
    </tt>    <tt>
    </tt>    object.should validate_length_of(<span class="sy">:attribute</span>, <span class="sy">:is</span> => <span class="i">5</span>)<tt>
    </tt>






## Views





My personal favorite matchers, you can now do stuff like:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>6<tt>
    </tt>7<tt>
    </tt>8<tt>
    </tt>9<tt>
    </tt><strong>10</strong><tt>
    </tt>11<tt>
    </tt>12<tt>
    </tt>


  
    
    <tt>
    </tt>    it <span class="s"><span class="dl">"</span><span class="k">should render new form</span><span class="dl">"</span></span> <span class="r">do</span><tt>
    </tt>        render <span class="s"><span class="dl">"</span><span class="k">/users/new.html.erb</span><span class="dl">"</span></span><tt>
    </tt><tt>
    </tt>        response.should have_form_posting_to(users_path) <span class="r">do</span><tt>
    </tt>          with_text_field_for(<span class="sy">:user_name</span>)<tt>
    </tt>          with_text_area_for(<span class="sy">:user_address</span>)<tt>
    </tt>          with_text_field_for(<span class="sy">:user_login</span>)<tt>
    </tt>          with_text_field_for(<span class="sy">:user_email</span>)<tt>
    </tt>          with_submit_button<tt>
    </tt>        <span class="r">end</span><tt>
    </tt>    <span class="r">end</span><tt>
    </tt>






Check the [readme](http://rspec-on-rails-matchers.googlecode.com/svn/trunk/README) for more information and details on the added matchers. I personally recommend you try the [TextMate Bundle](http://rspec-on-rails-matchers.googlecode.com/svn/textmate-bundle/RSpecOnRailsMatchers.tmbundle.zip) on top of being a perfect tool for lazy devs, it also lists all the available matchers and is an excellent way of learning.





We just released our first release yesterday, this is not a final version and we will keep on improving the code. If you have suggestions and patches feel free to open a ticket [there](http://code.google.com/p/rspec-on-rails-matchers/issues/lis).
