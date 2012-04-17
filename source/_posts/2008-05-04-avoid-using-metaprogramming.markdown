---
date: '2008-05-04 08:29:00'
layout: post
legacy_url: http://railsontherun.com/2008/05/04/avoid-using-metaprogramming/
slug: avoid-using-metaprogramming
source: railsontherun.com
status: publish
title: Avoid using metaprogramming (seriously!)
wordpress_id: '1128'
categories:
- Ruby
- railsontherun.com
- blog-post
tags:
- benchmark
- merb
- metaprogramming
- speed
---

Ruby is sexy, Ruby is cool and its metaprogramming potential offers some really cook features. However you might not realize that your cleverness is slowing down your code.

Today I was working on cleaning up merb_helper a Merb plugin that brings a lot of the stuff Rails developers are used to. In Merb we aim for speed and try to avoid magic.

merb_plugin didn't receive a lot of love from the main contributors but few features were added by different contributors and the code became hard to maintain.

Looking at the code I quickly found this bad boy:

(Old Merb Time DSL using metaprogramming)


    
    
    module MetaTimeDSL
    
        {:second => 1, 
         :minute => 60, 
         :hour => 3600, 
         :day => [24,:hours], 
         :week => [7,:days], 
         :month => [30,:days], 
         :year => [364.25, :days]}.each do |meth, amount|
          define_method "m_#{meth}" do
            amount = amount.is_a?(Array) ? amount[0].send(amount[1]) : amount
            self * amount
          end
          alias_method "m_#{meth}s".intern, "m_#{meth}"
        end
    
      end
      Numeric.send :include, MetaTimeDSL
    




The above code looks awful to me and I decided to rewrite it a way I thought would be more efficient:

    
    
     module TimeDSL
    
        def second
          self * 1
        end
        alias_method :seconds, :second
    
        def minute
          self * 60
        end
        alias_method :minutes, :minute
    
        def hour
          self * 3600
        end
        alias_method :hours, :hour
    
        def day
          self * 86400
        end
        alias_method :days, :day
    
        def week
          self * 604800
        end
        alias_method :weeks, :week
    
        def month
          self * 2592000
        end
        alias_method :months, :month
    
        def year
          self * 31471200
        end
        alias_method :years, :year
    
      end
      Numeric.send :include, TimeDSL
    



To make sure I was right, I run the following benchmarks:

    
    
    require 'benchmark'
    TIMES = (ARGV[0] || 100_000).to_i
    
    Benchmark.bmbm do |x|
      
      x.report("metaprogramming 360.seconds") do
        TIMES.times do    
          360.m_seconds
        end
      end
      
      x.report("no metaprogramming 360.hours") do
        TIMES.times do
          360.seconds
        end
      end
      
      x.report("metaprogramming 360.minutes") do
        TIMES.times do    
          360.m_minutes
        end
      end
      
      x.report("no metaprogramming 360.minutes") do
        TIMES.times do
          360.minutes
        end
      end
      
      x.report("metaprogramming 360.hours") do
        TIMES.times do    
          360.m_hours
        end
      end
      
      x.report("no metaprogramming 360.hours") do
        TIMES.times do
          360.hours
        end
      end
      
      x.report("metaprogramming 360.days") do
        TIMES.times do    
          360.m_days
        end
      end
      
      x.report("no metaprogramming 360.days") do
        TIMES.times do
          360.days
        end
      end
      
      x.report("metaprogramming 360.weeks") do
        TIMES.times do    
          360.m_weeks
        end
      end
      
      x.report("no metaprogramming 360.weeks") do
        TIMES.times do
          360.weeks
        end
      end
    
      x.report("metaprogramming 18.months") do
        TIMES.times do    
          18.m_months
        end
      end
      
      x.report("no metaprogramming 18.months") do
        TIMES.times do
          18.months
        end
      end
      
      x.report("metaprogramming 7.years") do
        TIMES.times do    
          7.m_years
        end
      end
      
      x.report("no metaprogramming 7.years") do
        TIMES.times do
          7.years
        end
      end
      
    end
    
    
     Rehearsal ------------------------------------------------------------------
    metaprogramming 360.seconds      0.130000   0.000000   0.130000 (  0.133164)
    no metaprogramming 360.hours     0.050000   0.000000   0.050000 (  0.042655)
    metaprogramming 360.minutes      0.130000   0.000000   0.130000 (  0.133327)
    no metaprogramming 360.minutes   0.040000   0.000000   0.040000 (  0.042401)
    metaprogramming 360.hours        0.140000   0.000000   0.140000 (  0.134312)
    no metaprogramming 360.hours     0.040000   0.000000   0.040000 (  0.043125)
    metaprogramming 360.days         0.130000   0.000000   0.130000 (  0.134949)
    no metaprogramming 360.days      0.050000   0.000000   0.050000 (  0.043745)
    metaprogramming 360.weeks        0.130000   0.000000   0.130000 (  0.135581)
    no metaprogramming 360.weeks     0.050000   0.000000   0.050000 (  0.043544)
    metaprogramming 18.months        0.130000   0.000000   0.130000 (  0.135234)
    no metaprogramming 18.months     0.050000   0.000000   0.050000 (  0.044354)
    metaprogramming 7.years          0.140000   0.000000   0.140000 (  0.144062)
    no metaprogramming 7.years       0.050000   0.000000   0.050000 (  0.044392)
    --------------------------------------------------------- total: 1.260000sec
    
                                         user     system      total        real
    metaprogramming 360.seconds      0.130000   0.000000   0.130000 (  0.132567)
    no metaprogramming 360.hours     0.040000   0.000000   0.040000 (  0.042777)
    metaprogramming 360.minutes      0.140000   0.000000   0.140000 (  0.132554)
    no metaprogramming 360.minutes   0.040000   0.000000   0.040000 (  0.043193)
    metaprogramming 360.hours        0.130000   0.000000   0.130000 (  0.133027)
    no metaprogramming 360.hours     0.050000   0.000000   0.050000 (  0.042613)
    metaprogramming 360.days         0.130000   0.000000   0.130000 (  0.138637)
    no metaprogramming 360.days      0.050000   0.000000   0.050000 (  0.043213)
    metaprogramming 360.weeks        0.130000   0.000000   0.130000 (  0.134049)
    no metaprogramming 360.weeks     0.040000   0.000000   0.040000 (  0.043713)
    metaprogramming 18.months        0.140000   0.000000   0.140000 (  0.134941)
    no metaprogramming 18.months     0.040000   0.000000   0.040000 (  0.043980)
    metaprogramming 7.years          0.150000   0.000000   0.150000 (  0.143389)
    no metaprogramming 7.years       0.040000   0.000000   0.040000 (  0.044585)
     0.136591)
    
    




The metaprogramming version of the same implementation is almost 3 times slower!

Moral of the story: be careful when using metaprogramming, you might end up slowing down your code considerably.
