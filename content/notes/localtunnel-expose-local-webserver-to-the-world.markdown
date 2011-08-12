title: LocalTunnel - Expose local web server to the world
author: Mir Nazim
published: 2011-08-12 17:00:00
tags: [share localhost, web server, tools, ruby, python]
public: yes
chronological : yes
kind: note
summary: |
    How many times did you hope there was a quick way to show the changes to your client quickly. Well LocalTunnel does exactly that.
...

***Code! Code! Code! Commit/Push. Deploy to staging. Send an email asking someone(somwhere else) to review changes.***

Recognize the scenario. What if the was a way to quickly show a web app running on your laptop/maching to a client or someone sitting somewhere else? Well now you can! All you need is this awesome tool called [LocalTunnel](http://progrium.com/localtunnel/) ([GitHub repository](http://github.com/progrium/localtunnel)).


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


