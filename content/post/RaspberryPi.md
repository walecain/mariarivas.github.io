+++
categories = ["Tutorials", "Raspberry Pi", "Tor"]
date = "2015-08-01T23:25:55+02:00"
tags = ["Tutorials", "Raspberry Pi", "Tor"]
title = "Convert Raspberry Pi into Tor Relay *not an exit node*"
draft = "false"

+++
<center>![RaspberryPi](/images/raspberry_pi.png)</center>

As an advocate for privacy, I am very enamored with the Tor Project, allowing users to retain their anonymity while using the Internet. And given that I have one of my Raspberry Pis lying around, I decided to help out the cause and convert my Pi into a Tor relay. My only complaint is with the speed of the Tor network, obviously because there are not enough volunteers donating their bandwidth, so this is my little way of helping out a great cause. 

## What is Tor?
<center>![Tor Logo](/images/tor-logo.png)</center>

Tor (an acronym for the original project called The Onion Router) is a free software that enables a second-generation onion routing system that allows for anonymous communications over a computer network. In an onion network, data sent through a network are enclosed in layers of encryption. These data are then transmitted to nodes (AKA onion routers) which strips a single layer of encryption at each stop, displaying where the data's next destination is. When the data has reached the target address, the last layer is stripped and decrypted. Each of the nodes involved in between never know the sender; rendering the sender anonymous. The only information available is the next destination of the data between the nodes. 

Unlike what the media and other laypersons may falsely claim, Tor is not used exclusively for "dark web" activities but as an essential tool for defense against the mass surveillance apparatus that threatens our personal freedom and privacy. 

#### Precautions for Exit Nodes

This tutorial is for a middle relay, the ones that transport data from the guard relay to the exit (the default factory settings for Tor goes through 3 relays). Running an exit node requires a bit more responsibility. These special nodes direct data straight to the target location of the sender, and so any illegal activities that come from the sender will show as if the exit relay was responsible for it, leaving the owner of this server with the possibility of being targeted by local authorities (in an extreme circumstance). If you would like to be adventurous and a true blessed soul, read up on Tor's site a [guide](https://blog.torproject.org/blog/tips-running-exit-node-minimal-harassment) for running an exit node with minimal harassment. 


## Setting up the Marvelous Pi
Being that I forgot my beloved HDMI cable on a trip during the summer, I have been forced to resort to creative means to connect to my Pi. I am using my TTL-serial cable to connect to the pi. For sake of simplicity, I will not mention how to do this as it is rather lengthy and it requires a tutorial for itself. [Here](http://workshop.raspberrypiaustralia.com/usb/ttl/connecting/2014/08/31/01-connecting-to-raspberry-pi-via-usb/) is a good explanation of it if that is your only means of connecting to your pi. So once the shell is brought up with the login prompt, we are ready to roll.

*Note: my Pi's OS is Raspbian.* 

*Irrelevant Note: Kali Linux has an ARM version wooohoo! Going to make the switch and have some real fun!*

#### Instructions
Let's install Tor first, no?
`apt-get install tor`

After it has been downloaded, you should cd into the torrc txt file to configure how your Tor will behave. If you compiled from src code then it will be located `/usr/local/etc/tor/torrc` and if you did not it should be at `/etc/tor/torrc or /etc/torrc`.

Uncomment and complete these lines in the section just for servers
```
#Nickname "a unique handle for this server"
#ContactInfo "<nobody@example.com>"
#ORPort 443
#ExitPolicy reject *:*
```
If you do not set any limits on bandwidth, it will be all taken hostage.
```
RelayBandwidthRate 300 KB
RelayBandwidthBurst 500 KB 
```

I use the anonymizing relay monitor (arm) tool to manage my Tor relay. Basically it's a terminal status monitor for Tor that provides live stats RE: bandwidth, CPU and memory usage along with connection details and configuration settings for your relay. Uncomment these lines and after I show a quick note on installing arm.
```
#ControlPort 9051
#CookieAuthentication 1
```

*Side Note: Installing arm; preferably to be done after the remaining steps are complete*

- - - - 
```
apt-get install tor-arm

#And then run it *after* you have finished the entire tutorial. 
sudo -u debian-tor arm
```
- - - -


Then we start up Tor.
```
/etc/init./tor start
```
Now we access all the currently active socket connections on the system via the wonderful netstat command coupled with grep. 
```
netstat -atnp | grep 443 
```

Check the Tor status and make sure that somewhere in there it states that the ORPort is reachable.  
```
tail /var/log/tor/log
```

So now you may check in [Atlas](https://atlas.torproject.org/) for the name you gave your server way back when we first configured. If it is there, then congrats, your relay is working!! But if not, just be aware that it takes a bit of time for the directories to be updated with brand spanking new server info. 

#### References & Links:
* [Tor](https://www.torproject.org/)
* [Arm Tool](https://www.atagar.com/index.php)
* [Netstat commands](http://www.binarytides.com/linux-netstat-command-examples/)
* [DevConsole](http://www.devconsole.info/?p=879)
* [How Tor Works](http://jordan-wright.com/blog/2015/02/28/how-tor-works-part-one/)
