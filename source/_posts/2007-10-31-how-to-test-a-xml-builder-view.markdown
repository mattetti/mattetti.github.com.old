---
date: '2007-10-31 08:32:00'
layout: post
legacy_url: http://railsontherun.com/2007/10/31/how-to-test-a-xml-builder-view/
slug: how-to-test-a-xml-builder-view
source: railsontherun.com
status: publish
title: How to test a XML builder view
wordpress_id: '169'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- BDD
- hpricot
- parser
- rails
- RSpec
- ruby
- TDD
- test
- view
- xml builder
---

As a good Rubyist, I do [TDD](http://en.wikipedia.org/wiki/Test-driven_development) and even [BDD](http://en.wikipedia.org/wiki/Behavior_driven_development). 





Since I've started using [RSpec](http://rspec.rubyforge.org/) I've started writing tests against my views. RSpec makes things really easy and I've been enjoying testing my views.





I'm not the only one having fun, check [this great post](http://www.robbyonrails.com/articles/2007/08/02/spec-your-views) from Mr [Planet Argon](http://www.planetargon.com/) aka [Robby Russel](http://www.robbyonrails.com)





Recently I was working on implementing some [Sexy Charts](http://railsontherun.com/2007/10/4/sexy-charts-in-less-than-5-minutes) and I was using a XML builder to create an XML view of for a controller. Since I wanted to be a good Rails Ninja and obey the BDD rules, I figured I needed to test my XML view. Making sure that the nodes and the attributes were properly created. Turned out that is wasn't too hard, there was many options but none were very well documented so I decided to write this quick tutorial.





## UPDATE 31 Oct 2007: After a comment from [Josh Knowles](http://joshknowles.com), I updated the tests to test with have_tags (built in RSpec) and hpricot.





## Hpricot





[hpricot](http://code.whytheluckystiff.net/hpricot/) is a awesome HTML parser perfect for [screen scraping](http://en.wikipedia.org/wiki/Screen_scraping). But wait, there's more to this awesome library, [hpricot can also parse XML](http://code.whytheluckystiff.net/hpricot/wiki/HpricotXML).





If you watched the excellent [RSpec peepcasts](http://peepcode.com/products) you probably noticed that [topfunky](http://topfunky.com/) aka [Geoffrey Grosenbach](http://geoffreygrosenbach.com/) uses hpricot to test a remote API.





In our case, we'll use hpricot to test that our generated XML follows our expectations.





## XML Builder + RSpec





Let's write a quick test to make sure our controller uses a XML builder view:






  
    
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
    </tt>


  
    
      describe <span class="co">AveragesController</span>, <span class="s"><span class="dl">"</span><span class="k">handling GET /averages.xml</span><span class="dl">"</span></span> <span class="r">do</span><tt>
    </tt><tt>
    </tt>    before <span class="r">do</span><tt>
    </tt>      <span class="co">Average</span>.stub!(<span class="sy">:find</span>).and_return(<span class="iv">@average</span>)<tt>
    </tt>    <span class="r">end</span><tt>
    </tt>  <tt>
    </tt>    <span class="r">def</span> <span class="fu">do_get</span><tt>
    </tt>      <span class="iv">@request</span>.env[<span class="s"><span class="dl">"</span><span class="k">HTTP_ACCEPT</span><span class="dl">"</span></span>] = <span class="s"><span class="dl">"</span><span class="k">application/xml</span><span class="dl">"</span></span><tt>
    </tt>      get <span class="sy">:index</span><tt>
    </tt>    <span class="r">end</span><tt>
    </tt>  <tt>
    </tt>    it <span class="s"><span class="dl">"</span><span class="k">should render the action using the XML builder</span><span class="dl">"</span></span> <span class="r">do</span><tt>
    </tt>      do_get<tt>
    </tt>      response.should render_template(<span class="s"><span class="dl">'</span><span class="k">averages/index.xml.builder</span><span class="dl">'</span></span>)<tt>
    </tt>    <span class="r">end</span><tt>
    </tt><tt>
    </tt>  <span class="r">end</span>






To make this example pass, we need to modify our rspec generated controller.






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>6<tt>
    </tt>7<tt>
    </tt>8<tt>
    </tt>


  
    
      <span class="r">def</span> <span class="fu">index</span><tt>
    </tt>    <span class="iv">@averages</span> = <span class="co">Average</span>.find(<span class="sy">:all</span>)<tt>
    </tt>  <tt>
    </tt>    respond_to <span class="r">do</span> |format|<tt>
    </tt>      format.html <span class="c"># index.html.erb</span><tt>
    </tt>      format.xml  { render <span class="sy">:action</span> => <span class="s"><span class="dl">"</span><span class="k">index.xml.builder</span><span class="dl">"</span></span>, <span class="sy">:layout</span> => <span class="pc">false</span> }<tt>
    </tt>    <span class="r">end</span><tt>
    </tt>  <span class="r">end</span>






(Please note that I'm using Rails 2.0 and that's why I'm not using a .rxml view)





Here is what our XML file should end up looking like:






  
    
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
    </tt>27<tt>
    </tt>28<tt>
    </tt>29<tt>
    </tt><strong>30</strong><tt>
    </tt>31<tt>
    </tt>32<tt>
    </tt>33<tt>
    </tt>34<tt>
    </tt><strong>35</strong><tt>
    </tt>36<tt>
    </tt>37<tt>
    </tt>38<tt>
    </tt>39<tt>
    </tt><strong>40</strong><tt>
    </tt>41<tt>
    </tt>42<tt>
    </tt>43<tt>
    </tt>44<tt>
    </tt><strong>45</strong><tt>
    </tt>46<tt>
    </tt>47<tt>
    </tt>48<tt>
    </tt>49<tt>
    </tt><strong>50</strong><tt>
    </tt>51<tt>
    </tt>52<tt>
    </tt>53<tt>
    </tt>54<tt>
    </tt>


  
    
      <<span class="i">?x</span>ml version=<span class="s"><span class="dl">"</span><span class="k">1.0</span><span class="dl">"</span></span> encoding=<span class="s"><span class="dl">"</span><span class="k">UTF-8</span><span class="dl">"</span></span>?><tt>
    </tt>  <chart><tt>
    </tt>    <series><tt>
    </tt>      <value xid=<span class="s"><span class="dl">"</span><span class="k">0</span><span class="dl">"</span></span>><span class="co">January</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt>      <value xid="1">February<</span><span class="dl">/</span></span>value><tt>
    </tt>      <value xid=<span class="s"><span class="dl">"</span><span class="k">2</span><span class="dl">"</span></span>><span class="co">March</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt>      <value xid="3">April<</span><span class="dl">/</span></span>value><tt>
    </tt>      <value xid=<span class="s"><span class="dl">"</span><span class="k">4</span><span class="dl">"</span></span>><span class="co">May</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt><tt>
    </tt>      <value xid="5">June<</span><span class="dl">/</span></span>value><tt>
    </tt>      <value xid=<span class="s"><span class="dl">"</span><span class="k">6</span><span class="dl">"</span></span>><span class="co">July</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt>      <value xid="7">August<</span><span class="dl">/</span></span>value><tt>
    </tt>      <value xid=<span class="s"><span class="dl">"</span><span class="k">8</span><span class="dl">"</span></span>><span class="co">September</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt>      <value xid="9">October<</span><span class="dl">/</span></span>value><tt>
    </tt>      <value xid=<span class="s"><span class="dl">"</span><span class="k">10</span><span class="dl">"</span></span>><span class="co">November</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt><tt>
    </tt>      <value xid="11">December<</span><span class="dl">/</span></span>value><tt>
    </tt>    <<span class="rx"><span class="dl">/</span><span class="k">series><tt>
    </tt>    <graphs><tt>
    </tt>      <graph fill_alpha="50" color="#FF0000" fill_color="#CC0000" title="high"><tt>
    </tt>        <value xid="0">65.1<</span><span class="dl">/</span></span>value><tt>
    </tt>        <value xid=<span class="s"><span class="dl">"</span><span class="k">1</span><span class="dl">"</span></span>><span class="fl">65.7</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt>        <value xid="2">64.9<</span><span class="dl">/</span></span>value><tt>
    </tt><tt>
    </tt>        <value xid=<span class="s"><span class="dl">"</span><span class="k">3</span><span class="dl">"</span></span>><span class="fl">66.7</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt>        <value xid="4">67.1<</span><span class="dl">/</span></span>value><tt>
    </tt>        <value xid=<span class="s"><span class="dl">"</span><span class="k">5</span><span class="dl">"</span></span>><span class="fl">69.3</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt>        <value xid="6">73.0<</span><span class="dl">/</span></span>value><tt>
    </tt>        <value xid=<span class="s"><span class="dl">"</span><span class="k">7</span><span class="dl">"</span></span>><span class="fl">74.8</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt>        <value xid="8">75.4<</span><span class="dl">/</span></span>value><tt>
    </tt><tt>
    </tt>        <value xid=<span class="s"><span class="dl">"</span><span class="k">9</span><span class="dl">"</span></span>><span class="fl">73.4</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt>        <value xid="10">68.9<</span><span class="dl">/</span></span>value><tt>
    </tt>        <value xid=<span class="s"><span class="dl">"</span><span class="k">11</span><span class="dl">"</span></span>><span class="fl">65.3</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt>      <</span><span class="dl">/</span></span>graph><tt>
    </tt>      <graph fill_alpha=<span class="s"><span class="dl">"</span><span class="k">50</span><span class="dl">"</span></span> color=<span class="s"><span class="dl">"</span><span class="k">#0000CC</span><span class="dl">"</span></span> fill_color=<span class="s"><span class="dl">"</span><span class="k">#0000CC</span><span class="dl">"</span></span> title=<span class="s"><span class="dl">"</span><span class="k">low</span><span class="dl">"</span></span>><tt>
    </tt>        <value xid=<span class="s"><span class="dl">"</span><span class="k">0</span><span class="dl">"</span></span>><span class="fl">48.9</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt>        <value xid="1">50.7<</span><span class="dl">/</span></span>value><tt>
    </tt><tt>
    </tt>        <value xid=<span class="s"><span class="dl">"</span><span class="k">2</span><span class="dl">"</span></span>><span class="fl">52.9</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt>        <value xid="3">55.6<</span><span class="dl">/</span></span>value><tt>
    </tt>        <value xid=<span class="s"><span class="dl">"</span><span class="k">4</span><span class="dl">"</span></span>><span class="fl">59.2</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt>        <value xid="5">61.9<</span><span class="dl">/</span></span>value><tt>
    </tt>        <value xid=<span class="s"><span class="dl">"</span><span class="k">6</span><span class="dl">"</span></span>><span class="fl">65.7</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt>        <value xid="7">67.3<</span><span class="dl">/</span></span>value><tt>
    </tt><tt>
    </tt>        <value xid=<span class="s"><span class="dl">"</span><span class="k">8</span><span class="dl">"</span></span>><span class="fl">65.7</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt>        <value xid="9">61.0<</span><span class="dl">/</span></span>value><tt>
    </tt>        <value xid=<span class="s"><span class="dl">"</span><span class="k">10</span><span class="dl">"</span></span>><span class="fl">54.0</span><<span class="rx"><span class="dl">/</span><span class="k">value><tt>
    </tt>        <value xid="11">48.7<</span><span class="dl">/</span></span>value><tt>
    </tt>      <<span class="rx"><span class="dl">/</span><span class="k">graph><tt>
    </tt>    <</span><span class="dl">/</span></span>graphs><tt>
    </tt>  <<span class="rx"><span class="dl">/</span><span class="k">chart><tt>
    </tt>  </span></span>






Let's write some tests to make sure our view is ok:





index.xml.builder_spec.rb






  
    
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
    </tt>27<tt>
    </tt>28<tt>
    </tt>29<tt>
    </tt><strong>30</strong><tt>
    </tt>31<tt>
    </tt>32<tt>
    </tt>33<tt>
    </tt>34<tt>
    </tt><strong>35</strong><tt>
    </tt>36<tt>
    </tt>37<tt>
    </tt>38<tt>
    </tt>39<tt>
    </tt><strong>40</strong><tt>
    </tt>41<tt>
    </tt>42<tt>
    </tt>43<tt>
    </tt>44<tt>
    </tt><strong>45</strong><tt>
    </tt>46<tt>
    </tt>47<tt>
    </tt>48<tt>
    </tt>49<tt>
    </tt><strong>50</strong><tt>
    </tt>51<tt>
    </tt>52<tt>
    </tt>53<tt>
    </tt>54<tt>
    </tt><strong>55</strong><tt>
    </tt>56<tt>
    </tt>57<tt>
    </tt>58<tt>
    </tt>59<tt>
    </tt><strong>60</strong><tt>
    </tt>61<tt>
    </tt>62<tt>
    </tt>63<tt>
    </tt>64<tt>
    </tt><strong>65</strong><tt>
    </tt>66<tt>
    </tt>67<tt>
    </tt>68<tt>
    </tt>69<tt>
    </tt><strong>70</strong><tt>
    </tt>71<tt>
    </tt>72<tt>
    </tt>73<tt>
    </tt>74<tt>
    </tt><strong>75</strong><tt>
    </tt>76<tt>
    </tt>77<tt>
    </tt>78<tt>
    </tt>79<tt>
    </tt><strong>80</strong><tt>
    </tt>81<tt>
    </tt>


  
    
    require <span class="co">File</span>.dirname(<span class="pc">__FILE__</span>) + <span class="s"><span class="dl">'</span><span class="k">/../../spec_helper</span><span class="dl">'</span></span><tt>
    </tt>require <span class="s"><span class="dl">'</span><span class="k">hpricot</span><span class="dl">'</span></span><tt>
    </tt><tt>
    </tt>describe <span class="s"><span class="dl">"</span><span class="k">/averages/index.xml.builder</span><span class="dl">"</span></span> <span class="r">do</span><tt>
    </tt>  include <span class="co">AveragesHelper</span><tt>
    </tt>  <tt>
    </tt>  before <span class="r">do</span><tt>
    </tt>    average_1 = mock_model(<span class="co">Average</span>)<tt>
    </tt>    average_1.stub!(<span class="sy">:month</span>).and_return(<span class="s"><span class="dl">"</span><span class="k">January</span><span class="dl">"</span></span>)<tt>
    </tt>    average_1.stub!(<span class="sy">:high</span>).and_return(<span class="s"><span class="dl">"</span><span class="k">74.5</span><span class="dl">"</span></span>)<tt>
    </tt>    average_1.stub!(<span class="sy">:low</span>).and_return(<span class="s"><span class="dl">"</span><span class="k">61.5</span><span class="dl">"</span></span>)<tt>
    </tt>    average_2 = mock_model(<span class="co">Average</span>)<tt>
    </tt>    average_2.stub!(<span class="sy">:month</span>).and_return(<span class="s"><span class="dl">"</span><span class="k">February</span><span class="dl">"</span></span>)<tt>
    </tt>    average_2.stub!(<span class="sy">:high</span>).and_return(<span class="s"><span class="dl">"</span><span class="k">82.5</span><span class="dl">"</span></span>)<tt>
    </tt>    average_2.stub!(<span class="sy">:low</span>).and_return(<span class="s"><span class="dl">"</span><span class="k">71.5</span><span class="dl">"</span></span>)<tt>
    </tt><tt>
    </tt>    assigns[<span class="sy">:averages</span>] = [average_1, average_2]<tt>
    </tt>  <span class="r">end</span><tt>
    </tt><tt>
    </tt>  it <span class="s"><span class="dl">"</span><span class="k">should render the months in the series</span><span class="dl">"</span></span> <span class="r">do</span><tt>
    </tt>    render <span class="s"><span class="dl">"</span><span class="k">/averages/index.xml.builder</span><span class="dl">"</span></span><tt>
    </tt>    response.should have_tag(<span class="s"><span class="dl">"</span><span class="k">value</span><span class="dl">"</span></span>, <span class="s"><span class="dl">'</span><span class="k">January</span><span class="dl">'</span></span>)<tt>
    </tt>    response.should have_tag(<span class="s"><span class="dl">"</span><span class="k">value</span><span class="dl">"</span></span>, <span class="s"><span class="dl">'</span><span class="k">February</span><span class="dl">'</span></span>)<tt>
    </tt>    <span class="c"># Same thing but with Hpricot</span><tt>
    </tt>    doc = <span class="co">Hpricot</span>.XML(response.body.to_s)<tt>
    </tt>    (doc/<span class="sy">:value</span>).first.inner_html.should == <span class="s"><span class="dl">'</span><span class="k">January</span><span class="dl">'</span></span><tt>
    </tt>    (doc/<span class="sy">:value</span>)[<span class="i">1</span>].inner_html.should == <span class="s"><span class="dl">'</span><span class="k">February</span><span class="dl">'</span></span><tt>
    </tt>  <span class="r">end</span><tt>
    </tt>  <tt>
    </tt>  it <span class="s"><span class="dl">"</span><span class="k">should set the xid attributes for the series</span><span class="dl">"</span></span> <span class="r">do</span><tt>
    </tt>    render <span class="s"><span class="dl">"</span><span class="k">/averages/index.xml.builder</span><span class="dl">"</span></span><tt>
    </tt>    response.should have_tag(<span class="s"><span class="dl">"</span><span class="k">value[xid=0]:first-child</span><span class="dl">"</span></span>)<tt>
    </tt>    response.should have_tag(<span class="s"><span class="dl">"</span><span class="k">value[xid=1]:last-child</span><span class="dl">"</span></span>)<tt>
    </tt>    <span class="c"># Same thing but with Hpricot</span><tt>
    </tt>    doc = <span class="co">Hpricot</span>.XML(response.body.to_s)<tt>
    </tt>    (doc/<span class="sy">:value</span>).first[<span class="s"><span class="dl">"</span><span class="k">xid</span><span class="dl">"</span></span>].should == <span class="s"><span class="dl">'</span><span class="k">0</span><span class="dl">'</span></span><tt>
    </tt>    (doc/<span class="sy">:value</span>).last[<span class="s"><span class="dl">"</span><span class="k">xid</span><span class="dl">"</span></span>].should == <span class="s"><span class="dl">'</span><span class="k">1</span><span class="dl">'</span></span><tt>
    </tt>  <span class="r">end</span><tt>
    </tt>  <tt>
    </tt>  it <span class="s"><span class="dl">"</span><span class="k">should have 2 graphs and they should have a title</span><span class="dl">"</span></span> <span class="r">do</span><tt>
    </tt>    render <span class="s"><span class="dl">"</span><span class="k">/averages/index.xml.builder</span><span class="dl">"</span></span><tt>
    </tt>    response.should have_tag(<span class="s"><span class="dl">"</span><span class="k">graph[title=high]:first-child</span><span class="dl">"</span></span>)<tt>
    </tt>    response.should have_tag(<span class="s"><span class="dl">"</span><span class="k">graph[title=low]:last-child</span><span class="dl">"</span></span>)<tt>
    </tt>    <span class="c"># Same thing but with Hpricot</span><tt>
    </tt>    doc = <span class="co">Hpricot</span>.XML(response.body.to_s)<tt>
    </tt>    (doc/<span class="sy">:graph</span>).size.should == <span class="i">2</span><tt>
    </tt>    (doc/<span class="sy">:graph</span>).first[<span class="s"><span class="dl">"</span><span class="k">title</span><span class="dl">"</span></span>].should == <span class="s"><span class="dl">'</span><span class="k">high</span><span class="dl">'</span></span><tt>
    </tt>    (doc/<span class="sy">:graph</span>).last[<span class="s"><span class="dl">"</span><span class="k">title</span><span class="dl">"</span></span>].should == <span class="s"><span class="dl">'</span><span class="k">low</span><span class="dl">'</span></span><tt>
    </tt>  <span class="r">end</span><tt>
    </tt>  <tt>
    </tt>  it <span class="s"><span class="dl">"</span><span class="k">should have a color set by graph</span><span class="dl">"</span></span> <span class="r">do</span><tt>
    </tt>    render <span class="s"><span class="dl">"</span><span class="k">/averages/index.xml.builder</span><span class="dl">"</span></span><tt>
    </tt>    response.should have_tag(<span class="s"><span class="dl">"</span><span class="k">graph[color]:first-child</span><span class="dl">"</span></span>)<tt>
    </tt>    response.should have_tag(<span class="s"><span class="dl">"</span><span class="k">graph[color]:last-child</span><span class="dl">"</span></span>)<tt>
    </tt>    response.should have_tag(<span class="s"><span class="dl">"</span><span class="k">graph[fill_color]:last-child</span><span class="dl">"</span></span>)<tt>
    </tt>    response.should have_tag(<span class="s"><span class="dl">"</span><span class="k">graph[fill_alpha]:last-child</span><span class="dl">"</span></span>)<tt>
    </tt>    <span class="c"># Same thing but with Hpricot</span><tt>
    </tt>    doc = <span class="co">Hpricot</span>.XML(response.body.to_s)<tt>
    </tt>    (doc/<span class="sy">:graph</span>).first[<span class="s"><span class="dl">"</span><span class="k">color</span><span class="dl">"</span></span>].should_not be_nil<tt>
    </tt>    (doc/<span class="sy">:graph</span>).last[<span class="s"><span class="dl">"</span><span class="k">color</span><span class="dl">"</span></span>].should_not be_nil<tt>
    </tt>    (doc/<span class="sy">:graph</span>).last[<span class="s"><span class="dl">"</span><span class="k">fill_color</span><span class="dl">"</span></span>].should_not be_nil<tt>
    </tt>    (doc/<span class="sy">:graph</span>).last[<span class="s"><span class="dl">"</span><span class="k">fill_alpha</span><span class="dl">"</span></span>].should_not be_nil<tt>
    </tt>  <span class="r">end</span><tt>
    </tt>  <tt>
    </tt>  it <span class="s"><span class="dl">"</span><span class="k">should have an xid for each graph value</span><span class="dl">"</span></span> <span class="r">do</span><tt>
    </tt>    render <span class="s"><span class="dl">"</span><span class="k">/averages/index.xml.builder</span><span class="dl">"</span></span><tt>
    </tt>    response.should have_tag(<span class="s"><span class="dl">"</span><span class="k">graph > value[xid=0]:first-child</span><span class="dl">"</span></span>)<tt>
    </tt>    <span class="c"># Same thing but with Hpricot</span><tt>
    </tt>    doc = <span class="co">Hpricot</span>.XML(response.body.to_s)<tt>
    </tt>    (doc/<span class="sy">:graph</span>/<span class="sy">:value</span>).first[<span class="s"><span class="dl">"</span><span class="k">xid</span><span class="dl">"</span></span>].should == <span class="s"><span class="dl">"</span><span class="k">0</span><span class="dl">"</span></span><tt>
    </tt>  <span class="r">end</span><tt>
    </tt>  <tt>
    </tt>  it <span class="s"><span class="dl">"</span><span class="k">should have the high average as values of the first graph</span><span class="dl">"</span></span> <span class="r">do</span><tt>
    </tt>    render <span class="s"><span class="dl">"</span><span class="k">/averages/index.xml.builder</span><span class="dl">"</span></span><tt>
    </tt>    response.should have_tag(<span class="s"><span class="dl">"</span><span class="k">graph > value:first-child</span><span class="dl">"</span></span>, <span class="s"><span class="dl">"</span><span class="k">74.5</span><span class="dl">"</span></span>)<tt>
    </tt>    <span class="c"># Same thing but with Hpricot</span><tt>
    </tt>    doc = <span class="co">Hpricot</span>.XML(response.body.to_s)<tt>
    </tt>    (doc/<span class="sy">:graph</span>/<span class="sy">:value</span>).first.inner_html.should == <span class="s"><span class="dl">"</span><span class="k">74.5</span><span class="dl">"</span></span><tt>
    </tt>  <span class="r">end</span><tt>
    </tt>  <tt>
    </tt><span class="r">end</span>






The first thing you must do (after installing the hpricot gem) is to require hpricot in your test:






  
    
    <tt>
    </tt>


  
    
      require <span class="s"><span class="dl">'</span><span class="k">hpricot</span><span class="dl">'</span></span>






Now that hpricot is created we can use it to parse the response and check against our expectations.





(we create mock objects to pass to the view so we know exactly what to expect and we separate Model/Controller/Views tests)





To check against our response we have to use hpricot parser syntax. It might look at bit funny at first, but believe me it's really easy once you get it.





But first, let's parse the view:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt>


  
    
    <span class="c"># Render the mocked up data using the xml view</span><tt>
    </tt>render <span class="s"><span class="dl">"</span><span class="k">/averages/index.xml.builder</span><span class="dl">"</span></span><tt>
    </tt><span class="c"># Load and parse the view response body:</span><tt>
    </tt>doc = <span class="co">Hpricot</span>.XML(response.body.to_s)  






Let's look at the first test:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>6<tt>
    </tt>


  
    
      it <span class="s"><span class="dl">"</span><span class="k">should render the months in the series</span><span class="dl">"</span></span> <span class="r">do</span><tt>
    </tt>    render <span class="s"><span class="dl">"</span><span class="k">/averages/index.xml.builder</span><span class="dl">"</span></span><tt>
    </tt>    doc = <span class="co">Hpricot</span>.XML(response.body.to_s)<tt>
    </tt>    (doc/<span class="sy">:value</span>).first.inner_html.should == <span class="s"><span class="dl">'</span><span class="k">January</span><span class="dl">'</span></span><tt>
    </tt>    (doc/<span class="sy">:value</span>)[<span class="i">1</span>].inner_html.should == <span class="s"><span class="dl">'</span><span class="k">February</span><span class="dl">'</span></span><tt>
    </tt>  <span class="r">end</span>






(doc/:value) returns all the value nodes, we take the first one and extract its content. We expect that it would match the name of the month for the first average.





Let's now look at another test:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>6<tt>
    </tt>7<tt>
    </tt>


  
    
      it <span class="s"><span class="dl">"</span><span class="k">should have 2 graphs and they should have a title</span><span class="dl">"</span></span> <span class="r">do</span><tt>
    </tt>    render <span class="s"><span class="dl">"</span><span class="k">/averages/index.xml.builder</span><span class="dl">"</span></span><tt>
    </tt>    doc = <span class="co">Hpricot</span>.XML(response.body.to_s)<tt>
    </tt>    (doc/<span class="sy">:graph</span>).size.should == <span class="i">2</span><tt>
    </tt>    (doc/<span class="sy">:graph</span>).first[<span class="s"><span class="dl">"</span><span class="k">title</span><span class="dl">"</span></span>].should == <span class="s"><span class="dl">'</span><span class="k">high</span><span class="dl">'</span></span><tt>
    </tt>    (doc/<span class="sy">:graph</span>).last[<span class="s"><span class="dl">"</span><span class="k">title</span><span class="dl">"</span></span>].should == <span class="s"><span class="dl">'</span><span class="k">low</span><span class="dl">'</span></span><tt>
    </tt>  <span class="r">end</span>






The thing to look at here is the fact that we are checking on the node's attribute "title". Really simple syntax and clean test, isn't it?





Finally let's look at the last example:






  
    
    1<tt>
    </tt>2<tt>
    </tt>3<tt>
    </tt>4<tt>
    </tt><strong>5</strong><tt>
    </tt>


  
    
      it <span class="s"><span class="dl">"</span><span class="k">should have the high average as values of the first graph</span><span class="dl">"</span></span> <span class="r">do</span><tt>
    </tt>    render <span class="s"><span class="dl">"</span><span class="k">/averages/index.xml.builder</span><span class="dl">"</span></span><tt>
    </tt>    doc = <span class="co">Hpricot</span>.XML(response.body.to_s)<tt>
    </tt>    (doc/<span class="sy">:graph</span>/<span class="sy">:value</span>).first.inner_html.should == <span class="s"><span class="dl">"</span><span class="k">74.5</span><span class="dl">"</span></span><tt>
    </tt>  <span class="r">end</span>






We are checking that the content of the first value node nested inside a graph node is equal to 74.5 which is the high average for the first month. 





In practice, you probably won't write all these tests at once, but anyway, let's look at our XML builder which will make all these tests pass:






  
    
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
    </tt>


  
    
    xml.instruct! <span class="sy">:xml</span>, <span class="sy">:version</span>=><span class="s"><span class="dl">"</span><span class="k">1.0</span><span class="dl">"</span></span>, <span class="sy">:encoding</span>=><span class="s"><span class="dl">"</span><span class="k">UTF-8</span><span class="dl">"</span></span><tt>
    </tt>xml.chart <span class="r">do</span><tt>
    </tt>  xml.series <span class="r">do</span>    <tt>
    </tt>    <span class="iv">@averages</span>.each_with_index <span class="r">do</span> |average, index|<tt>
    </tt>      xml.value average.month, <span class="sy">:xid</span> => index<tt>
    </tt>    <span class="r">end</span><tt>
    </tt>  <span class="r">end</span><tt>
    </tt>  <tt>
    </tt>  xml.graphs <span class="r">do</span><tt>
    </tt>    xml.graph <span class="sy">:title</span> => <span class="s"><span class="dl">'</span><span class="k">high</span><span class="dl">'</span></span>, <span class="sy">:color</span> => <span class="s"><span class="dl">"</span><span class="k">#FF0000</span><span class="dl">"</span></span>, <span class="sy">:fill_alpha</span> => <span class="s"><span class="dl">"</span><span class="k">50</span><span class="dl">"</span></span>, <span class="sy">:fill_color</span> => <span class="s"><span class="dl">"</span><span class="k">#CC0000</span><span class="dl">"</span></span> <span class="r">do</span><tt>
    </tt>      <span class="iv">@averages</span>.each_with_index <span class="r">do</span> |average, index|<tt>
    </tt>        xml.value average.high, <span class="sy">:xid</span> => index<tt>
    </tt>      <span class="r">end</span><tt>
    </tt>    <span class="r">end</span><tt>
    </tt>    <tt>
    </tt>    xml.graph <span class="sy">:title</span> => <span class="s"><span class="dl">'</span><span class="k">low</span><span class="dl">'</span></span>, <span class="sy">:color</span> => <span class="s"><span class="dl">"</span><span class="k">#0000CC</span><span class="dl">"</span></span>, <span class="sy">:fill_alpha</span> => <span class="s"><span class="dl">"</span><span class="k">50</span><span class="dl">"</span></span>, <span class="sy">:fill_color</span> => <span class="s"><span class="dl">"</span><span class="k">#0000CC</span><span class="dl">"</span></span> <span class="r">do</span><tt>
    </tt>      <span class="iv">@averages</span>.each_with_index <span class="r">do</span> |average, index|<tt>
    </tt>        xml.value average.low, <span class="sy">:xid</span> => index<tt>
    </tt>      <span class="r">end</span><tt>
    </tt>    <span class="r">end</span><tt>
    </tt>  <span class="r">end</span><tt>
    </tt>  <tt>
    </tt><span class="r">end</span>






Hpricot is a _really_ nice tool which can make your BDD life much easier. And even if you don't do BDD/TDD yet, it's a great way to verify that any XML data you receive/generate is valid.





## Happy testing
