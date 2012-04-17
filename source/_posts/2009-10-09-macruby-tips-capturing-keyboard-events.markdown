---
date: '2009-10-09 19:24:04'
layout: post
legacy_url: http://merbist.com/2009/10/09/macruby-tips-capturing-keyboard-events/
slug: macruby-tips-capturing-keyboard-events
source: merbist.com
status: publish
title: 'MacRuby tips: capturing keyboard events'
wordpress_id: '602'
categories:
- macruby
- Tutorial
- merbist.com
- blog-post
---

If you are writing any type of games you might want your users to interact with your application using their keyboards.

This is actually not that hard. The approach is simple and fast forward if you are used to Cocoa.

Everything starts in Interface Builder, add a custom view instance to your window.

![](http://img.skitch.com/20091010-8tf834wf9se6y81h2jf7a7e5he.jpg)

Now switch to your project and a new file with a class called KeyboardControlView and make in inherit from NSView. We are creating a subview of NSView so we will be able to make our top view "layer" use this subclass.

    
    class KeyboardControlView < NSView
      attr_accessor :game_controller
    
      def acceptsFirstResponder
        true
      end
    
    end


As you can see in the example above, I added an attribute accessor. attr_accessor class method creates getters and setters. It's basically the same as writing:

    
     def game_controller=(value)
      @game_controller = value
    end
    
    def game_controller
      @game_controller
    end


MacRuby is keeping an eye on these accessors and let bind outlets to them.
But let's not get ahead of ourselves, we'll keep that for another time.

Let's go back to our newly created class. Notice, we also added a method called `
[acceptsFirstResponder](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ApplicationKit/Classes/NSResponder_Class/Reference/Reference.html#//apple_ref/occ/instm/NSResponder/acceptsFirstResponder)` and returns true. [acceptsFirstResponder](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ApplicationKit/Classes/NSResponder_Class/Reference/Reference.html#//apple_ref/occ/instm/NSResponder/acceptsFirstResponder) returns false by default.
But in this case we want it to return true so our new class instance can be first in the responder chain.

Now that our class is ready, let's go back to Interface Builder, select our new custom view and click on the inspector button.

![](http://img.skitch.com/20091010-1trkxhw2r2paaik3ipa4gtytgg.jpg)
Click on the (i) icon and in the Class field choose our new KeyboardControlView.
Yep, our new class just shows up by magic, it's also called the lrz effect, just don't ask ;)
So now when our application starts, a new instance of our NSView class is created and Cocoa will call different methods based on events triggered.

The two methods we are interested in reimplementing are keyDown and keyUp. They get called when a key gets pressed or released.

    
    def keyDown(event)
      characters = event.characters
      if characters.length == 1 && !event.isARepeat
        character = characters.characterAtIndex(0)
        if character == NSLeftArrowFunctionKey
          puts "LEFT pressed"
        elsif character == NSRightArrowFunctionKey
          puts "RIGHT pressed"
        elsif character == NSUpArrowFunctionKey
          puts "UP pressed"
        elsif character == NSDownArrowFunctionKey
          puts "DOWN pressed"
        end
      end
     super
    end


I don't think the code above needs much explanation. The only things that you might not understand are 'event.isARepeat'. This method returns true if the user left his/her finger on the key. The other thing is the use of the 'super' call at the end of the method. Basically, we reopened a method that was already defined and we don't want to just overwrite it, we just want to inject out code within the existing method, so once we are done handling the event, we just pass it back to original method.

Final result:

    
    class KeyboardControlView < NSView
      attr_accessor :game_controller
    
      def acceptsFirstResponder
        true
      end
    
      def keyDown(event)
        characters = event.characters
        if characters.length == 1 && !event.isARepeat
          character = characters.characterAtIndex(0)
          if character == NSLeftArrowFunctionKey
            puts "LEFT pressed"
          elsif character == NSRightArrowFunctionKey
            puts "RIGHT pressed"
          elsif character == NSUpArrowFunctionKey
            puts "UP pressed"
          elsif character == NSDownArrowFunctionKey
      	puts "DOWN pressed"
          end
        end
        super
      end
    
      # Deals with keyboard keys being released
      def keyUp(event)
        characters = event.characters
        if characters.length == 1
          character = characters.characterAtIndex(0)
          if character == NSLeftArrowFunctionKey
           puts "LEFT released"
          elsif character == NSRightArrowFunctionKey
            puts "RIGHT released"
          elsif character == NSUpArrowFunctionKey
           puts "UP released"
          elsif character == NSDownArrowFunctionKey
            puts "DOWN released"
          end
        end
        super
      end
    
    end


Now it's up to you to handle the other keystrokes and do whatever you want. That's it for this tip, I hope it helps.
