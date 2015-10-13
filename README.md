# wasp

WASP x64 ( Windows x64 , Apache 2.4 x64 , SVN, PHP )
To make things easy WASP will now go out in the wild.

Ever wanted a 64 bit distribution of apache 2.4, svn and php but putting all the parts together caused butt pain?
Well here you go.

How stable is it?
It hasn't crashed in production. YET.

How is it configured?
httpd.conf. It uses lots of memory! It eats 2.3 GB of RAM, and then holds steady due to a 2GB svn cache. If you have a small repo and more RAM? MAKE the cache BIGGER!

In production it currently serves a 1.6TB svn repo with 465k revisions that is growing, and acts as a proxy for multiple internal web servers. It also runs PHP so you can use it to do other useful things. Uses rotatelogs to make daily rotating logs, cause usually no one sets this up either!

Why did I put it together?
Because nothing out there fit my needs.

Why did I put this here?
Because hopefully it'll benefit someone else that also wants to roll their own and not use collabnet/wandisco/visualsvn. They are all great products, but not quite what I'm looking for.

How do I install it?
Extract to C:\WASP. Run httpd.exe