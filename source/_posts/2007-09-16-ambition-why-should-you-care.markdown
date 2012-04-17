---
date: '2007-09-16 01:51:00'
layout: post
legacy_url: http://railsontherun.com/2007/09/16/ambition-why-should-you-care/
slug: ambition-why-should-you-care
source: railsontherun.com
status: publish
title: Ambition, why should you care?
wordpress_id: '92'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- activeRecord
- ambition
- gem
- ORM
---

![ambition](http://errtheblog.com/static/images/ambition-tower.png)





By now, you should have heard about [ambition](http://errtheblog.com/post/10722) if not [read the latest post from the author](http://errtheblog.com/post/11998).





**Ambition has a simple goal: making you stop writing SQL in your queries and only stick to Ruby. (who cares if you use ActiveRecord, Sequel, DataMapper or another ORM)**





I'm so used to the ActiveRecord way of querying the database that I was not fully convinced that Ambition would help me in my daily tasks. I still gave it a try: 





## Testing Ambition




    
    <code>$ sudo gem install ambition -y
    </code>





Started my console




    
    <code>$ script/console
    </code>





and required Ambition




    
    <code>require 'ambition'
    </code>





I started by doing a query the AR way:






  
    
    1<tt>
    </tt>2<tt>
    </tt>


  
    
     <span class="co">Photo</span>.find(<span class="sy">:all</span>, <span class="sy">:conditions</span> => <span class="s"><span class="dl">"</span><span class="k">photos.title IS NULL AND photos.width > 250 <tt>
    </tt>AND photos.height > 200 AND users.name = 'test'</span><span class="dl">"</span></span>, <span class="sy">:include</span> => <span class="sy">:user</span>) 






And I converted it into an Ambition call:






  
    
    <tt>
    </tt>


  
    
    <span class="co">Photo</span>.select {|p| p.title == <span class="pc">nil</span> && p.width > <span class="i">250</span> && p.height > <span class="i">200</span>  && p.user.name == <span class="s"><span class="dl">'</span><span class="k">test</span><span class="dl">'</span></span>}.entries






145 vs 102 keystrokes. 30% less typing with Ambition! I don't know about you, but I REALLY prefer the Ruby only query, much cleaner and much "DRYer". However, that's not always true:






  
    
    <tt>
    </tt>


  
    
    <span class="co">Photo</span>.find_by_title(<span class="pc">nil</span>)






(24chars) 


  
    
    <tt>
    </tt>


  
    
    <span class="co">Photo</span>.detect{|p| p.title == <span class="pc">nil</span>}



 (32 chars)





But what's going on behind the scene? Do we have the exact same SQL query sent to our DB?





Well, Ambition doesn't generate any SQL, it uses AR to do so. You want to make sure Ambition is not messing with you, try that:






  
    
    1<tt>
    </tt>2<tt>
    </tt>


  
    
     >> <span class="co">Photo</span>.select {|p| p.title == <span class="pc">nil</span> && p.width > <span class="i">250</span> && p.height > <span class="i">200</span>  && p.user.name == <span class="s"><span class="dl">'</span><span class="k">test</span><span class="dl">'</span></span>}.to_hash<tt>
    </tt> => {<span class="sy">:conditions</span>=><span class="s"><span class="dl">"</span><span class="k">(photos.`title` IS NULL AND (photos.`width` > 250 AND (photos.`height` > 200 AND users.name = 'test')))</span><span class="dl">"</span></span>, <span class="sy">:include</span>=>[<span class="sy">:user</span>]}






That's pretty hot. Especially when you have to use eager loading! 





Obviously you can still do stuff like that:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>


  
    
    <span class="co">Photo</span>.select {|p| p.title == <span class="pc">nil</span> && p.width > <span class="i">250</span> && p.height > <span class="i">200</span>  && p.user.name == <span class="s"><span class="dl">'</span><span class="k">test</span><span class="dl">'</span></span>}.each <span class="r">do</span> |photo|<tt>
    </tt> puts photo.filename<tt>
    </tt><span class="r">end</span>






(note the query will only be made once)





Another cool thing, is to do simple sorting:






  
    
    <tt>
    </tt>


  
    
    >> <span class="co">Photo</span>.select {|p| p.title == <span class="pc">nil</span> && p.user.name == <span class="s"><span class="dl">'</span><span class="k">test</span><span class="dl">'</span></span>}.sort_by { |p| [p.created_at, -p.size] }






creates the following:    






  
    
    <tt>
    </tt>


  
    
    => {<span class="sy">:order</span>=><span class="s"><span class="dl">"</span><span class="k">photos.created_at, photos.size DESC</span><span class="dl">"</span></span>, <span class="sy">:conditions</span>=><span class="s"><span class="dl">"</span><span class="k">(photos.`title` IS NULL AND users.name = 'test')</span><span class="dl">"</span></span>, <span class="sy">:include</span>=>[<span class="sy">:user</span>]}






or    






  
    
    <tt>
    </tt>


  
    
    => <span class="s"><span class="dl">"</span><span class="k">SELECT * FROM photos JOIN user WHERE (photos.`title` IS NULL AND users.name = 'test') ORDER BY photos.created_at, photos.size DESC</span><span class="dl">"</span></span>






That's cool, and you can still sort on relationships:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>6<tt>
    </tt>7<tt>
    </tt>


  
    
    >> <span class="co">Photo</span>.select {|p| p.title == <span class="pc">nil</span> }.sort_by { |p| p.user.name }<tt>
    </tt>=> <span class="s"><span class="dl">"</span><span class="k">SELECT * FROM photos JOIN user WHERE photos.`title` IS NULL ORDER BY users.name</span><span class="dl">"</span></span><<span class="rx"><span class="dl">/</span><span class="k">macro:code ><tt>
    </tt>    <tt>
    </tt>Or directly on the model:<tt>
    </tt><tt>
    </tt><macro:code lang="ruby">>> Photo.sort_by(&:title)<tt>
    </tt>=> "SELECT * FROM photos ORDER BY photos.title"</span></span>






To finish, another detail which makes Ambition a great library






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>


  
    
    >> <span class="co">Photo</span>.any? {|p| p.title =~ <span class="rx"><span class="dl">/</span><span class="k">ambition</span><span class="dl">/</span></span> }<tt>
    </tt>=> <span class="s"><span class="dl">"</span><span class="k">SELECT count(*) AS count_all FROM photos WHERE (photos.`title` REGEXP 'ambition')</span><span class="dl">"</span></span> <tt>
    </tt>=> <span class="pc">true</span>






And if you were worried that it wouldn't work with utf8, check this out:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>6<tt>
    </tt>


  
    
    >> <span class="co">Photo</span>.any? {|p| p.title == <span class="s"><span class="dl">'</span><span class="k">Ã©cole</span><span class="dl">'</span></span>}<tt>
    </tt>=> <span class="co">SET</span> <span class="co">NAMES</span> <span class="s"><span class="dl">'</span><span class="k">utf8</span><span class="dl">'</span></span><tt>
    </tt>=> <span class="co">SET</span> <span class="co">SQL_AUTO_IS_NULL</span>=<span class="i">0</span><tt>
    </tt>=> <span class="co">SHOW</span> <span class="co">FIELDS</span> <span class="co">FROM</span> photos<tt>
    </tt>=> <span class="co">SELECT</span> count(*) <span class="co">AS</span> count_all <span class="co">FROM</span> photos <span class="co">WHERE</span> (photos.`title<span class="sh"><span class="dl">`</span><span class="k"> = 'Ã©cole') <tt>
    </tt>=> false</span></span>






## Limitations





The only limitation I found in Ambition is that Ruby code won't work in the block, for instance:






  
    
    <tt>
    </tt>


  
    
    >> <span class="co">Photo</span>.select {|p| p.title == <span class="pc">nil</span> && p.created_at < <span class="i">1</span>.week.ago && p.user.name == <span class="s"><span class="dl">'</span><span class="k">test</span><span class="dl">'</span></span>}.entries






won't work at the moment. To inspect what's going simply try:






  
    
    1<tt>
    </tt>2<tt>
    </tt>


  
    
    >> <span class="co">Photo</span>.select {|p| p.title == <span class="pc">nil</span> && p.created_at < <span class="i">1</span>.week.ago && p.user.name == <span class="s"><span class="dl">'</span><span class="k">test</span><span class="dl">'</span></span>}.to_sql<tt>
    </tt>=> <span class="s"><span class="dl">"</span><span class="k">SELECT * FROM photos JOIN user WHERE (photos.`title` IS NULL AND (photos.`created_at` < 1.`week`.`ago` AND users.name = 'test'))</span><span class="dl">"</span></span>






You can see that **photos.`created_at` < 1.`week`.`ago`**  is the problem.





The recommended way to achieve the same result is to use variables:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>


  
    
    >> date = <span class="i">1</span>.week.ago<tt>
    </tt>>> <span class="co">Photo</span>.select {|p| p.title == <span class="pc">nil</span> && p.created_at < date && p.user.name == <span class="s"><span class="dl">'</span><span class="k">test</span><span class="dl">'</span></span>}.to_sql<tt>
    </tt>=> <span class="s"><span class="dl">"</span><span class="k">SELECT * FROM photos JOIN user WHERE (photos.`title` IS NULL AND (photos.`created_at` < '2007-09-08 19:38:48' AND users.name = 'test'))</span><span class="dl">"</span></span>






However, note that method calls will work just fine:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>6<tt>
    </tt>


  
    
    >> <span class="r">def</span> <span class="fu">time_now_please</span><tt>
    </tt>>> <span class="co">Time</span>.now<tt>
    </tt>>> <span class="r">end</span><tt>
    </tt>    <tt>
    </tt>>> <span class="co">Photo</span>.select {|p| p.title == <span class="pc">nil</span> && p.created_at < time_now_please && p.user.name == <span class="s"><span class="dl">'</span><span class="k">test</span><span class="dl">'</span></span>}.to_sql<tt>
    </tt>=> <span class="s"><span class="dl">"</span><span class="k">SELECT * FROM photos JOIN user WHERE (photos.`title` IS NULL AND (photos.`created_at` < '2007-09-15 19:41:37' AND users.name = 'test'))</span><span class="dl">"</span></span>   






## Conclusion





For now, Ambition is still just wrapping ActiveRecord::Base#find but the plan is to actually generate SQL. Hopefully we'll also be able to use Ruby code from within an Ambition block. Kickers methods are very interesting and could become a really nice way of speeding up your app and keep your code clean.





Ambition is a great query library, I think I'll start using it whenever I have "find" calls with multiple conditions especially if my conditions are related to another model. However I still didn't figure out how to use an inner join with Ambition.
