.. _tutorial-starting_varnish:


Starting Varnish
----------------

This tutorial will assume that you are either running Ubuntu, Debian,
Red Hat Enterprise Linux or Centos. Those of you running on other
platforms might have to do some mental translation exercises in order
to follow me. Since you're on a "weird" platform you're probably used
to it. :-)

I assume you have Varnish Cache installed.

You start Varnish with ``service varnish start``.  This will start
Varnish if it isn't already running.

Now you have Varnish running. Let us make sure that it works
properly. Use your browser to go to http://127.0.0.1:6081/
(obviously, you should replace the IP address with one on your own
system). 

The default configuration will try to forward requests to a web
service running on the same computer as Varnish was installed at,
port 8080.

If there is no web application being served up there Varnish will
issue an error. Varnish Cache is very conservative about telling the
world what is wrong so whenever something is amiss it will issue the
same generic "Error 503 Service Unavilable".

You might have a web application running on some other port or some
other computer. Let's edit the configuration and make it point to
something that actually works.

Fire up your favorite editor and edit /etc/varnish/default.vcl. Most
of it is commented out but there is some text that is not. It will
probably look like this::

  backend default {
      .host = "127.0.0.1";
      .port = "8080";
  }

We'll change it and make it point to something that works. Hopefully
www.varnish-cache.org is up. Let's use that. Replace the text with::

  backend default {
      .host = "www.varnish-cache.org";
      .port = "80";
  }


Now issue ``service varnish reload`` to make Varnish reload it's
configuration. If that succeeded visit http://127.0.0.1:6081/ in your
browser and you should see some directory listing. It works! The
reason you're not seeing the Varnish official website is because your
client isn't sending the appropriate Host: header in the request and
it ends up showing a listing of the default webfolder on the machine
usually serving up varnish-cache.org.
