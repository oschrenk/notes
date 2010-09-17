# Privoxy + Tor #

The bundle that the Tor project offers [online](http://www.torproject.org/easy-download.html.en) offers a gui, Tor itself and according to [this](http://www.torproject.org/download.html.en) also [Privoxy](http://www.privoxy.org/). Opening the package contents or just a quick glance at the settings reveals that it is bundled with [Polipo](http://www.pps.jussieu.fr/~jch/software/polipo/), another web proxy.

Let's get the sources and configure it ourselves:

## Privoxy ##

    $ sudo port install privoxy
    $ sudo port installed | grep tor
      privoxy @3.0.12_0+darwin (active)

The macport installation installs Privoxy as user `privoxy:privoxy`, so if you try to start it by just calling

    $ privoxy

you get something like

    Jan 14 13:30:58.313 000002f8 Fatal error: can't open configuration file '/opt/local/etc/privoxy/config':  Permission denied

call

    $ sudo -u privoxy privoxy

instead.

Now you can use Privoxy as your Proxy. To use it in your favorite terminal, use

    $ export http_proxy=http://127.0.0.1:8118/

To use it on Firefox open the preferences (`Cmd + ,`) and click through to `Advanced > Network > Settings... > Manual proxy configuration` and enter the above address. To use it in Safari you have to hange your network settings as Safari uses the system proxy settings.

You can forward to Tor by editing the config file

    $ sudo vi /opt/local/etc/privoxy/config

and uncomment the line

    forward-socks4a   /               127.0.0.1:9050 .

You can stop privoxy by calling

    $ sudo killall privoxy

To check if privoxy is running just open <http://config.privoxy.org/>
in your browser.

It also features a [status page](http://config.privoxy.org/show-status) where you can see which configuration options are being used.

## Tor ##

    $ sudo port install tor
    $ sudo port installed | grep tor
    $ tor

You get something like

    Jan 14 15:38:42.336 [notice] Tor v0.2.1.21. This is experimental software. Do not rely on it for strong anonymity. (Running on Darwin i386)
    Jan 14 15:38:42.337 [notice] Configuration file "/opt/local/etc/tor/torrc" not present, using reasonable defaults.
    Jan 14 15:38:42.337 [notice] Initialized libevent version 1.4.13-stable using method kqueue. Good.
    Jan 14 15:38:42.337 [notice] Opening Socks listener on 127.0.0.1:9050
    Jan 14 15:38:42.362 [notice] Parsing GEOIP file.
    Jan 14 15:38:43.109 [notice] We now have enough directory information to build circuits.
    Jan 14 15:38:43.109 [notice] Bootstrapped 80%: Connecting to the Tor network.
    Jan 14 15:38:43.140 [notice] Bootstrapped 85%: Finishing handshake with first hop.
    Jan 14 15:38:43.300 [notice] Bootstrapped 90%: Establishing a Tor circuit.
    Jan 14 15:38:45.562 [notice] Tor has successfully opened a circuit. Looks like client functionality is working.
    Jan 14 15:38:45.562 [notice] Bootstrapped 100%: Done.

### Source ###

I had no luck getting the source via git

    git clone git://git.torproject.org/git/tor

The server was not reachable, even the [source tree](https://git.torproject.org/checkout/tor/master/) wasn't. But it should be available