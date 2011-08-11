title: LocalTunnel - Expose local web server to the world
author: Mir Nazim
published: 2011-05-07 17:57:00
tags: [share localhost, web server, tools, ruby, python]
public: yes
chronological : yes
kind: note
summary: |
    How many times did you hope there was a quick way to show the changes to your client quickly. Well LocalTunnel does exactly that.
...

 - Code! Code! Code!
 - Commit. Push.
 - Deploy to staging.
 - Send email to client to review changes.

Recognize the scenario. What if you wanted to a way to quickly show a web app on your laptop/maching to client or someone else? Well now you can! All you need is the awesome tool called [LocalTunnel](http://progrium.com/localtunnel/) ([GitHub](http://github.com/progrium/localtunnel))

### Installation on Ubuntu

Install the requirements(ruby, openssl etc.)
    :::bash
    $ sudo apt-get install ruby ruby1.8-dev rubygems1.8 libopenssl-ruby 

Install LocalTunnel itself
    :::bash
    $ gem install localtunnel

Generate an ssh key(if not done already)
    :::bash
    $ ssh-keygen -t rsa

Expose a the port for your local web-server
    :::bash
    $ localtunnel -k ~/.ssh/id_rsa.pub 8080

You will get a public url like xyz.localtunnel.com for one time use. Enjoy!


