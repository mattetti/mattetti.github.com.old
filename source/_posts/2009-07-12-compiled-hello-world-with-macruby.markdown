---
date: '2009-07-12 11:53:53'
layout: post
legacy_url: http://merbist.com/2009/07/12/compiled-hello-world-with-macruby/
slug: compiled-hello-world-with-macruby
source: merbist.com
status: publish
title: Compiled hello world with MacRuby
wordpress_id: '519'
categories:
- macruby
- Misc
- merbist.com
- blog-post
tags:
- compiler
- experimental
- llvm
- macruby
---

To celebrate the amazing [work being done by Laurent Sansonetti on MacRuby](http://lists.macosforge.org/pipermail/macruby-devel/2009-July/002062.html) here is a hello world using the new [LLVM](http://en.wikipedia.org/wiki/LLVM) based compiler.
`
$ echo "p ARGV.join(' ').upcase" > hello_world.rb
$ macrubyc hello_world.rb -o macruby_says
$ ./macruby_says hello world
"HELLO WORLD"
`

Note that to achieve this result, you need to be using the experimental branch of MacRuby and have LLVM installed. (check the [readme](http://svn.macosforge.org/repository/ruby/MacRuby/branches/experimental/README.rdoc) available in MacRuby's repo).

Let's quickly look at what we just did.
We created a single line ruby script that takes the passed arguments, joins them and print them uppercase.
Then, we compiled our script into a [Mach-O](http://en.wikipedia.org/wiki/Mach-O) object file and produce an executable.

Here is an extract from Laurent's latest status report:


> Produced executables embed all the compiled Ruby code as well as MacRuby, statically.
It can be distributed as is and does not depend on anything MacRuby or LLVM at runtime.
The Ruby source code is compiled into native machine code (same process as we do at runtime with the JIT compiler), so it's also a good way to obfuscate the source code.
The final binary looks like an Objective-C binary (except that it's larger)


Don't expect to compile Rails just yet, it's still in a preliminary stage.

The final release you should let you pick one of the 2 compilation modes:

* **normal mode**: full ruby specs, compile down to machine code and use LLVM at runtime. (recommended for desktop/server apps)
* **full mode**: no full ruby spec support, no runtime code generation, no LLVM. ("very light application and/or if the environment does not support runtime code generation" (_hint-hint)_)

As you can see, [MacRuby](http://macruby.org) is moving forward and the experimental branch should soon move to trunk.
