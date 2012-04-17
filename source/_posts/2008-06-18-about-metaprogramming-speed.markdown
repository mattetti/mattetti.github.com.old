---
date: '2008-06-18 07:03:00'
layout: post
legacy_url: http://railsontherun.com/2008/06/18/about-metaprogramming-speed/
slug: about-metaprogramming-speed
source: railsontherun.com
status: publish
title: About Metaprogramming speed
wordpress_id: '1245'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- benchmarks
- metaprogramming
- ruby
---

In a [previous article](http://railsontherun.com/2008/5/4/avoid-using-metaprogramming) I took an example of bad metaprogramming and I pushed people to think twice before using metaprogramming.





My main points were that:







  * you might make your code way slower if you don't know what you are doing


  * readability might drop considerably


  * maintainability can become an issue





People left some very good comments about how to write the same module using metaprogramming and keep things fast.





Today [Wycats](http://yehudakatz.com) pinged me about this post and told me that the issue was define_method and that class_eval is effectively the same as regular code, it gets evaluated in eval.c, just like regular Ruby code. On the other hand, defined_method has to marshall the proc.





I cleaned up my benchmarks using [rbench](http://github.com/somebee/rbench/tree/master), added some of the solutions provided to me and obtained the following results:





![results](http://img.skitch.com/20080618-ju8hy1b1pw8c3hb882ksr3hbed.jpg)





Here is the original/bad metaprogramming example:






  
    
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
    </tt>


  
    
    <tt>
    </tt>  <span class="r">module</span> <span class="cl">MetaTimeDSL</span><tt>
    </tt><tt>
    </tt>    {<span class="sy">:second</span> => <span class="i">1</span>, <tt>
    </tt>     <span class="sy">:minute</span> => <span class="i">60</span>, <tt>
    </tt>     <span class="sy">:hour</span> => <span class="i">3600</span>, <tt>
    </tt>     <span class="sy">:day</span> => [<span class="i">24</span>,<span class="sy">:hours</span>], <tt>
    </tt>     <span class="sy">:week</span> => [<span class="i">7</span>,<span class="sy">:days</span>], <tt>
    </tt>     <span class="sy">:month</span> => [<span class="i">30</span>,<span class="sy">:days</span>], <tt>
    </tt>     <span class="sy">:year</span> => [<span class="fl">364.25</span>, <span class="sy">:days</span>]}.each <span class="r">do</span> |meth, amount|<tt>
    </tt>      define_method <span class="s"><span class="dl">"</span><span class="il"><span class="dl">#{</span>meth<span class="dl">}</span></span><span class="dl">"</span></span> <span class="r">do</span><tt>
    </tt>        amount = amount.is_a?(<span class="co">Array</span>) ? amount[<span class="i">0</span>].send(amount[<span class="i">1</span>]) : amount<tt>
    </tt>        <span class="pc">self</span> * amount<tt>
    </tt>      <span class="r">end</span><tt>
    </tt>      alias_method <span class="s"><span class="dl">"</span><span class="il"><span class="dl">#{</span>meth<span class="dl">}</span></span><span class="k">s</span><span class="dl">"</span></span>.intern, <span class="s"><span class="dl">"</span><span class="il"><span class="dl">#{</span>meth<span class="dl">}</span></span><span class="dl">"</span></span><tt>
    </tt>    <span class="r">end</span><tt>
    </tt><tt>
    </tt>  <span class="r">end</span><tt>
    </tt>  <span class="co">Numeric</span>.send <span class="sy">:include</span>, <span class="co">MetaTimeDSL</span><tt>
    </tt>






The no metaprogramming module is available [there](http://pastie.textmate.org/217046)





Refactored:






  
    
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
    </tt>


  
    
    <tt>
    </tt>  <span class="r">module</span> <span class="cl">RefaMetaTimeDSL</span><tt>
    </tt><tt>
    </tt>  {<span class="sy">:second</span> => <span class="i">1</span>, <tt>
    </tt>   <span class="sy">:minute</span> => <span class="i">60</span>, <tt>
    </tt>   <span class="sy">:hour</span> => <span class="i">3600</span>, <tt>
    </tt>   <span class="sy">:day</span> => [<span class="i">24</span>,<span class="sy">:hours</span>], <tt>
    </tt>   <span class="sy">:week</span> => [<span class="i">7</span>,<span class="sy">:days</span>], <tt>
    </tt>   <span class="sy">:month</span> => [<span class="i">30</span>,<span class="sy">:days</span>], <tt>
    </tt>   <span class="sy">:year</span> => [<span class="fl">364.25</span>, <span class="sy">:days</span>]}.each <span class="r">do</span> |meth, amount|<tt>
    </tt>    <span class="pc">self</span>.class_eval <span class="s"><span class="dl"><<-RUBY</span></span><span class="s"><span class="k"><tt>
    </tt>      def r_</span><span class="il"><span class="dl">#{</span>meth<span class="dl">}</span></span><span class="k"><tt>
    </tt>        </span><span class="il"><span class="dl">#{</span>amount.is_a?(<span class="co">Array</span>) ? <span class="s"><span class="dl">"</span><span class="il"><span class="dl">#{</span>amount[<span class="i">0</span>]<span class="dl">}</span></span><span class="k">.</span><span class="il"><span class="dl">#{</span>amount[<span class="i">1</span>]<span class="dl">}</span></span><span class="dl">"</span></span> : <span class="s"><span class="dl">"</span><span class="il"><span class="dl">#{</span>amount<span class="dl">}</span></span><span class="dl">"</span></span><span class="dl">}</span></span><span class="k"><tt>
    </tt>      end<tt>
    </tt>      alias_method :r_</span><span class="il"><span class="dl">#{</span>meth<span class="dl">}</span></span><span class="k">s, :r_</span><span class="il"><span class="dl">#{</span>meth<span class="dl">}</span></span><span class="dl"><tt>
    </tt>    RUBY</span></span><tt>
    </tt>  <span class="r">end</span><tt>
    </tt><tt>
    </tt><span class="r">end</span><tt>
    </tt><span class="co">Numeric</span>.send <span class="sy">:include</span>, <span class="co">RefaMetaTimeDSL</span><tt>
    </tt>






the refactor 2 or eval based solution provided by [Matt Jones](http://pastie.caboo.se/191414) which uses class_eval like the previous refactor.






  
    
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
    </tt>


  
    
    <tt>
    </tt>  <span class="r">module</span> <span class="cl">EvalMetaTimeDSL</span><tt>
    </tt>    <span class="r">def</span> <span class="pc">self</span>.included(base)<tt>
    </tt>      base.class_eval <span class="r">do</span><tt>
    </tt>        [ [<span class="sy">:e_second</span>, <span class="i">1</span>], <tt>
    </tt>          [<span class="sy">:e_minute</span>, <span class="i">60</span>], <tt>
    </tt>          [<span class="sy">:e_hour</span>, <span class="i">3600</span>], <tt>
    </tt>          [<span class="sy">:e_day</span>, [<span class="i">24</span>,<span class="sy">:e_hours</span>]], <tt>
    </tt>          [<span class="sy">:e_week</span>, [<span class="i">7</span>,<span class="sy">:e_days</span>]], <tt>
    </tt>          [<span class="sy">:e_month</span>, [<span class="i">30</span>,<span class="sy">:e_days</span>]], <tt>
    </tt>          [<span class="sy">:e_year</span>, [<span class="fl">365.25</span>, <span class="sy">:e_days</span>]]].each <span class="r">do</span> |meth, amount|<tt>
    </tt>            amount = amount.is_a?(<span class="co">Array</span>) ? amount[<span class="i">0</span>].send(amount[<span class="i">1</span>]) : amount<tt>
    </tt>            eval <span class="s"><span class="dl">"</span><span class="k">def </span><span class="il"><span class="dl">#{</span>meth<span class="dl">}</span></span><span class="k">; self*</span><span class="il"><span class="dl">#{</span>amount<span class="dl">}</span></span><span class="k">; end</span><span class="dl">"</span></span><tt>
    </tt>            alias_method <span class="s"><span class="dl">"</span><span class="il"><span class="dl">#{</span>meth<span class="dl">}</span></span><span class="k">s</span><span class="dl">"</span></span>, meth<tt>
    </tt>          <span class="r">end</span><tt>
    </tt>      <span class="r">end</span><tt>
    </tt>    <span class="r">end</span><tt>
    </tt>  <span class="r">end</span><tt>
    </tt>  <span class="co">Numeric</span>.send <span class="sy">:include</span>, <span class="co">EvalMetaTimeDSL</span><tt>
    </tt>






and finally, the "better metaprogramming" version:






  
    
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
    </tt>


  
    
    <tt>
    </tt> <span class="r">module</span> <span class="cl">GoodMetaTimeDSL</span><tt>
    </tt><tt>
    </tt>  <span class="co">SECOND</span>  = <span class="i">1</span><tt>
    </tt>  <span class="co">MINUTE</span>  = <span class="co">SECOND</span> * <span class="i">60</span><tt>
    </tt>  <span class="co">HOUR</span>    = <span class="co">MINUTE</span> * <span class="i">60</span><tt>
    </tt>  <span class="co">DAY</span>     = <span class="co">HOUR</span> * <span class="i">24</span><tt>
    </tt>  <span class="co">WEEK</span>    = <span class="co">DAY</span> * <span class="i">7</span><tt>
    </tt>  <span class="co">MONTH</span>   = <span class="co">DAY</span> * <span class="i">30</span><tt>
    </tt>  <span class="co">YEAR</span>    = <span class="co">DAY</span> * <span class="fl">364.25</span><tt>
    </tt>  <tt>
    </tt>  <span class="s"><span class="dl">%w[</span><span class="k">SECOND MINUTE HOUR DAY WEEK MONTH YEAR</span><span class="dl">]</span></span>.each <span class="r">do</span> |const_name|<tt>
    </tt>      meth = const_name.downcase<tt>
    </tt>      class_eval <span class="s"><span class="dl"><<-RUBY</span></span> <span class="s"><span class="k"><tt>
    </tt>        def g_</span><span class="il"><span class="dl">#{</span>meth<span class="dl">}</span></span><span class="k"> <tt>
    </tt>          self * </span><span class="il"><span class="dl">#{</span>const_name<span class="dl">}</span></span><span class="k"> <tt>
    </tt>        end <tt>
    </tt>        alias g_</span><span class="il"><span class="dl">#{</span>meth<span class="dl">}</span></span><span class="k">s g_</span><span class="il"><span class="dl">#{</span>meth<span class="dl">}</span></span><span class="k"> </span><span class="dl"><tt>
    </tt>      RUBY</span></span><tt>
    </tt>  <span class="r">end</span><tt>
    </tt><span class="r">end</span><tt>
    </tt><span class="co">Numeric</span>.send <span class="sy">:include</span>, <span class="co">GoodMetaTimeDSL</span><tt>
    </tt>






Looking at the refactored version by Wycats, you can see he's right and the major issue with the original version was define_method. Using class_eval does make things almost as fast and even faster than the no metaprogramming version.





Interesting enough, the benchmarks show that some methods from the meta modules are faster than the ones from the no meta module. Overall, an optimized metaprogramming can be more or else as fast as a non meta code. Of course, with the new VMs coming up, things might change a little bit depending on the language implementation.





_In conclusion, metaprogramming can be as fast as no metaprogramming but that won't help your code readability and maintainability, so make sure to only use this great trick when needed!_





p.s: [here](http://pastie.textmate.org/217071) is the benchmark file if you don't believe me ;)
