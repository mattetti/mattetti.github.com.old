---
date: '2008-01-30 08:00:00'
layout: post
legacy_url: http://railsontherun.com/2008/01/30/misc-tips-and-tricks/
slug: misc-tips-and-tricks
source: railsontherun.com
status: publish
title: Misc tips and tricks
wordpress_id: '509'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- autotest
- BDD
- integration
- RSpec
- testing
- zentest
---

I haven't posted for quite a long time. The thing is I moved to a new place and I'm really busy on working clients + setting up my new office + dealing with way too much paperwork. 





Anyway, enough excuses, here are few tips that I believe will be useful to some of you:





## [ZenTest Autotest](http://www.zenspider.com/ZSS/Products/ZenTest/)





I love autotest, but you might have noticed that sometimes (especially on big projects), ZenTest might start using more CPU than expected. On my machine, that results in the fan going off and annoying the crap out of me.





The solution is quite simple, exclude all folders you don't need to monitor. To do that, update ZenTest to version 3.8.X 




    
    <code>sudo gem update ZenTest
    </code>





(older version had a different syntax)





Now, edit your .autotest that should be located in ~/.autotest  (if it doesn't exist, create it).





Finally add the following code:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt>


  
    
    <tt>
    </tt>  <span class="co">Autotest</span>.add_hook <span class="sy">:initialize</span> <span class="r">do</span> |at|<tt>
    </tt>    <span class="s"><span class="dl">%w{</span><span class="k">.svn .hg .git vendor</span><span class="dl">}</span></span>.each {|exception| at.add_exception(exception)}<tt>
    </tt>  <span class="r">end</span><tt>
    </tt>






I personally freeze rails in vendor and I autotest is way happier when it doesn't have to monitor some extra files. (note that we also exclude folders such as .git or .svn)
(you can also include files etc... read more [there](http://blog.davidchelimsky.net/articles/2008/01/15/rspec-1-1-2-and-zentest-3-8-0))





## [RSpec](http://rspec.info)





RSpec is certainly my favorite Ruby tool and I'm glad to say that most of my [SD.rb](http://sdruby.com/) friends finally got convinced!





Now, few people complained to me about spec failures outputting the full stack such as:






  
    
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
    </tt>13<tt>
    </tt>14<tt>
    </tt><strong>15</strong><tt>
    </tt>16<tt>
    </tt>17<tt>
    </tt>18<tt>
    </tt>19<tt>
    </tt><strong>20</strong><tt>
    </tt>21<tt>
    </tt>22<tt>
    </tt>23<tt>
    </tt>24<tt>
    </tt><strong>25</strong><tt>
    </tt>26<tt>
    </tt>


  
    
    <tt>
    </tt> <span class="co">The</span> <span class="co">Sessions</span> controller should fail since it<span class="s"><span class="dl">'</span><span class="k">s a test</span><span class="dl">'</span></span> <span class="co">FAILED</span><tt>
    </tt> expected <span class="pc">true</span>, got <span class="pc">false</span><tt>
    </tt> test_app-git/trunk/vendor/plugins/rspec/lib/spec/expectations.rb:<span class="i">52</span><span class="sy">:in</span> <span class="sh"><span class="dl">`</span><span class="k">fail_with'<tt>
    </tt> test_app-git/trunk/vendor/plugins/rspec/lib/spec/expectations/handler.rb:21:in </span><span class="dl">`</span></span>handle_matcher<span class="s"><span class="dl">'</span><span class="k"><tt>
    </tt> test_app-git/trunk/vendor/plugins/rspec/lib/spec/expectations/extensions/object.rb:34:in `should</span><span class="dl">'</span></span><tt>
    </tt> ./spec/controllers/sessions_controller_spec.rb:<span class="i">25</span>:<tt>
    </tt> test_app-git/trunk/vendor/plugins/rspec/lib/spec/example/example_methods.rb:<span class="i">78</span><span class="sy">:in</span> <span class="sh"><span class="dl">`</span><span class="k">instance_eval'<tt>
    </tt> test_app-git/trunk/vendor/plugins/rspec/lib/spec/example/example_methods.rb:78:in </span><span class="dl">`</span></span>run_with_description_capturing<span class="s"><span class="dl">'</span><span class="k"><tt>
    </tt> test_app-git/trunk/vendor/plugins/rspec/lib/spec/example/example_methods.rb:19:in `execute</span><span class="dl">'</span></span><tt>
    </tt> <span class="rx"><span class="dl">/</span><span class="k">opt</span><span class="dl">/</span></span>local/lib/ruby/<span class="fl">1.8</span>/timeout.rb:<span class="i">48</span><span class="sy">:in</span> <span class="sh"><span class="dl">`</span><span class="k">timeout'<tt>
    </tt> test_app-git/trunk/vendor/plugins/rspec/lib/spec/example/example_methods.rb:16:in </span><span class="dl">`</span></span>execute<span class="s"><span class="dl">'</span><span class="k"><tt>
    </tt> test_app-git/trunk/vendor/plugins/rspec/lib/spec/example/example_group_methods.rb:288:in `execute_examples</span><span class="dl">'</span></span><tt>
    </tt> test_app-git/trunk/vendor/plugins/rspec/lib/spec/example/example_group_methods.rb:<span class="i">287</span><span class="sy">:in</span> <span class="sh"><span class="dl">`</span><span class="k">each'<tt>
    </tt> test_app-git/trunk/vendor/plugins/rspec/lib/spec/example/example_group_methods.rb:287:in </span><span class="dl">`</span></span>execute_examples<span class="s"><span class="dl">'</span><span class="k"><tt>
    </tt> test_app-git/trunk/vendor/plugins/rspec/lib/spec/example/example_group_methods.rb:121:in `run</span><span class="dl">'</span></span><tt>
    </tt> test_app-git/trunk/vendor/plugins/rspec/lib/spec/runner/example_group_runner.rb:<span class="i">22</span><span class="sy">:in</span> <span class="sh"><span class="dl">`</span><span class="k">run'<tt>
    </tt> test_app-git/trunk/vendor/plugins/rspec/lib/spec/runner/example_group_runner.rb:21:in </span><span class="dl">`</span></span>each<span class="s"><span class="dl">'</span><span class="k"><tt>
    </tt> test_app-git/trunk/vendor/plugins/rspec/lib/spec/runner/example_group_runner.rb:21:in `run</span><span class="dl">'</span></span><tt>
    </tt> test_app-git/trunk/vendor/plugins/rspec/lib/spec/runner/options.rb:<span class="i">89</span><span class="sy">:in</span> <span class="sh"><span class="dl">`</span><span class="k">run_examples'<tt>
    </tt> test_app-git/trunk/vendor/plugins/rspec/lib/spec/runner/command_line.rb:19:in </span><span class="dl">`</span></span>run<span class="s"><span class="dl">'</span><span class="k"><tt>
    </tt> script/spec:4:<tt>
    </tt><tt>
    </tt>  Finished in 6.035147 seconds<tt>
    </tt><tt>
    </tt>  400 examples, 1 failure<tt>
    </tt></span></span>






We can really easily change that, open you spec.opts file located in your spec folder.





it probably looks like that:






  
    
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
    </tt>  --colour<tt>
    </tt>  --format<tt>
    </tt>  progress<tt>
    </tt>  --loadby<tt>
    </tt>  mtime<tt>
    </tt>  --reverse<tt>
    </tt>  --backtrace<tt>
    </tt>






Get rid of "--backtrace" and your new failure should look like:






  
    
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
    </tt>


  
    
    <tt>
    </tt>  <span class="i">1</span>)<tt>
    </tt>  <span class="s"><span class="dl">'</span><span class="k">The Sessions controller The Sessions controller should fail since it</span><span class="dl">'</span></span>s a test<span class="s"><span class="dl">'</span><span class="k"> FAILED<tt>
    </tt>  expected false, got true<tt>
    </tt>  ./spec/controllers/sessions_controller_spec.rb:25:<tt>
    </tt>  script/spec:4:<tt>
    </tt><tt>
    </tt>  Finished in 0.269956 seconds<tt>
    </tt><tt>
    </tt>  15 examples, 1 failure<tt>
    </tt>  <tt>
    </tt></span></span>






## Other stuff you may find interesting (in no particular order):







  * [spec converter](http://opensource.thinkrelevance.com/wiki/spec-converter)


  * [Rails Cell](http://cells.rubyforge.org/overview.html)


  * [Git to manage and deploy a Rails app](http://jointheconversation.org/railsgit)


  * [contacts (retrieve user's contacts from yahoo, gmail etc..)](http://rufy.com/contacts/doc/)


  * [Hashrocket](http://www.hashrocket.com/)


  * [one of the best Rails book of the moment](http://www.amazon.com/Rails-Way-Addison-Wesley-Professional-Ruby/dp/0321445619)


  * [err's new baby](http://famspam.com/)


  * [caboose conf 08](http://blog.caboo.se/articles/2008/1/30/caboose-conf-2008)


  * [git hub](http://github.com/)


  * [SWX Ruby (or how to get Rails to talk with Flash even faster)](http://swxruby.org/)


  * [Ruby Reddit](http://ruby.reddit.com)


