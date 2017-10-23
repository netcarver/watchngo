watchngo
========

A php-native inotify-based file watcher to easily trigger compilation. Debounces and coalesces changes to multiple files related to the same desired action.

Background
==========

We have a large app with 4-5 different things that need "watch & recompile" functionality (compass, coffeescript, etc). 

I spent *hours* trying to get various solutions like guard working but was never able to get them configured to work properly. Also in many cases they didn't notice events on certain machines, I think due to the different save mechanisms of say vim vs TextMate.

Besides, I am sick of having to use ruby/node projects to manage tasks in my PHP codebase, and am trying to help the PHP community have access to more robust tooling.

* guard for php
* grunt for php

Features
========

* Configure as many different "groups" as you want
* Specify a glob of files to monitor
* Specify an action to perform when a change is detected in the monitored files
* Each groups' various watches are coalseced and de-bounced, such that if you *save all* on various files in a project, the action will run only once, and only after the *last* detected change, debounced at 2s

Status
======

The script completley works, but it's obviously terrible code. I was just testing out the basic parts as fast as possible.

Going forward I'd like to refactor this into a proper CLI app with a config file, and possible plugins which can generate configs for common projects like (ie a compass generator could inspect your compass configs and auto-generate watchngo configs).

Prerequisites/Setup
===================

You need PHP installed with the inotify extension. The extension is installed by PECL and then compiled, so you need the PHPize dependency. To set this up on a Debian/Mint based system [(see original source)](https://askubuntu.com/questions/885574/how-does-one-install-the-pecl-inotify-extension-for-php7#885575), as root...

    # apt-get install php-dev
    # pecl install inotify
    
You now need to create and enable the inotify .so extension to PHP...

    # touch /etc/php/7.0/mods-available/inotify.ini
    # echo 'extension=inotify.so' > /etc/php/7.0/mods-available/inotify.ini
    # phpenmod inotify

A run of ```php -m``` should now show ```inotify``` as one of the installed modules.


