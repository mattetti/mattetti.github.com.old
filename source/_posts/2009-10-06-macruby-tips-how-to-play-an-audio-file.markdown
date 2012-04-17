---
date: '2009-10-06 19:00:08'
layout: post
legacy_url: http://merbist.com/2009/10/06/macruby-tips-how-to-play-an-audio-file/
slug: macruby-tips-how-to-play-an-audio-file
source: merbist.com
status: publish
title: 'MacRuby tips: how to play an audio file'
wordpress_id: '570'
categories:
- macruby
- merbist.com
- blog-post
tags:
- audio
- cocoa
- macruby
- NSSound
---

Let's say you would like to play an audio file in your MacRuby app/script, how would you do?
It's actually pretty simple, you just need to use NSSound. If we wanted to use a system sound we could do:

    
    NSSound.soundNamed('Basso').play


But let's look at a more advanced example with some Cocoa patterns. We will loop through all the audio files in a folder and we will play them one after the other.

    
    #!/usr/bin/env macruby
    framework 'Cocoa'
    # Cocoa documentation reference:
    # http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSound_Class/Reference/Reference.html
    
    def play_sound
      if @sounds.empty?
        NSApplication.sharedApplication.terminate(nil)
      else
        sound_file = @sounds.shift
        s = NSSound.alloc.initWithContentsOfFile(sound_file, byReference: false)
        puts "previewing #{sound_file}"
        s.delegate = self
        s.play
      end
    end
    
    # This is a delegate method called by the sound object
    def sound(sound, didFinishPlaying: state)
      play_sound if state
    end
    
    # Delegate method called when the app finished loading
    def applicationDidFinishLaunching(notification)
      @sounds = Dir.glob("/System/Library/Sounds/*.aiff")
      play_sound
    end
    
    # We are delegating the application to self so the script will know when
    # it finished loading
    NSApplication.sharedApplication.delegate = self
    NSApplication.sharedApplication.run


On my machine, I get the following output:

    
    $ macruby macrubysound.rb
    previewing /System/Library/Sounds/Basso.aiff
    previewing /System/Library/Sounds/Blow.aiff
    previewing /System/Library/Sounds/Bottle.aiff
    previewing /System/Library/Sounds/Frog.aiff
    previewing /System/Library/Sounds/Funk.aiff
    previewing /System/Library/Sounds/Glass.aiff
    previewing /System/Library/Sounds/Hero.aiff
    previewing /System/Library/Sounds/Morse.aiff
    previewing /System/Library/Sounds/Ping.aiff
    previewing /System/Library/Sounds/Pop.aiff
    previewing /System/Library/Sounds/Purr.aiff
    previewing /System/Library/Sounds/Sosumi.aiff
    previewing /System/Library/Sounds/Submarine.aiff
    previewing /System/Library/Sounds/Tink.aiff


Cocoa delegation might seem a bit strange at first when you come from Ruby. In Ruby we rarely do any type of async delegation so let's quickly look at what's going on in this script.

The first thing we do is loading the cocoa framework which makes total sense, then we define a bunch of methods and finally we get to:

    
    NSApplication.sharedApplication.delegate = self
    NSApplication.sharedApplication.run


We are setting the run loop to delegate to set, which means that when an event is triggered, the delegation method will be called on self (our script).
Once that done, we are starting out run loop.

Once the application is loaded the applicationDidFinishLaunching delegate is being triggered (line 24). At this point, we are looking for all the sound files in the sound system folder and storing them in an instance variable (line 25). Finally, we are calling the play_sound method (line 26).

The play_sound method checks that we have some audio files left to play (line 7), otherwise it quits the app. (line 8 ) If we still have some files in the queue, we get the first one (line 10) and use it to create an instance of NSSound (line 11).

Before playing the NSSound instance, we are setting its delegate to our script (self) (line 13). If you read [NSSound Cocoa documentation](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSound_Class/Reference/Reference.html) for #play, you will notice that #play "initiates playback asynchronously and returns control to your application. Therefore, your application can continue doing work while the audio is playing." In our case, we don't want to do that, otherwise all the sounds will play at the same time.

The documentation also mentions a delegation you can use to avoid that. This is exactly what we did when we implemented:  def sound(sound, didFinishPlaying: state) (line 19)

Rubyists might be surprised by the method signature. MacRuby extended Ruby to support Objective-C selectors.

When the sound is playing, this delegate gets called, we check on the state of the sound, if it finished playing then we call play_sound again.

That's it!  It's a very elegant implementation giving us the benefits of both Ruby and Cocoa.
