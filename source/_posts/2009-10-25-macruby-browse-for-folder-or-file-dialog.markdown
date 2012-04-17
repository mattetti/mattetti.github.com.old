---
date: '2009-10-25 15:52:14'
layout: post
legacy_url: http://merbist.com/2009/10/25/macruby-browse-for-folder-or-file-dialog/
slug: macruby-browse-for-folder-or-file-dialog
source: merbist.com
status: publish
title: 'MacRuby tips: browse for folder or file dialog'
wordpress_id: '624'
categories:
- macruby
- Tutorial
- merbist.com
- blog-post
tags:
- cocoa
- macruby
- tip
---

This is yet another pretty simple tip. 
Use case: let say you want your applications users to choose one or multiple files or folder on their file system. A good example would be that you want the user to choose a file to process or a folder where to save some data.

![](http://img.skitch.com/20091025-nc89xd2ywqutqqddnwm2met3x4.jpg)

In the example above, I added a browse button and a text field.

I would like my users to click on the browse button, locate a folder and display it in the text field.

In your MacRuby controller, use a simple action method as well as an accessor to the text field:


    
    
    attr_accessor :destination_path
    
    def browse(sender)
    end
    



Now, in Interface builder bind the destination_path outlet to the text field you want to use to display the path and bind the button to the browse action.

Let's go back to our action method and let's create a dialog panel, set some options and handle the user selection:


    
    
    def browse(sender)
      # Create the File Open Dialog class.
      dialog = NSOpenPanel.openPanel
      # Disable the selection of files in the dialog.
      dialog.canChooseFiles = false
      # Enable the selection of directories in the dialog.
      dialog.canChooseDirectories = true
      # Disable the selection of multiple items in the dialog.
      dialog.allowsMultipleSelection = false
    
      # Display the dialog and process the selected folder
      if dialog.runModalForDirectory(nil, file:nil) == NSOKButton
      # if we had a allowed for the selection of multiple items
      # we would have want to loop through the selection
        destination_path.stringValue = dialog.filenames.first
      end
    end
    



That's it, your user can now browse for a folder and the selection will be displayed in the text field. Look at the [NSOpenPanel documentation](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOpenPanel_Class/Reference/Reference.html) for more details on the Cocoa API.
