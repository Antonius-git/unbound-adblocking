# unbound-adblocking
What this Is:
-------------
A nice juicy list of things to adblock using the illustrious Unbound DNS.
While Unbound is nice on linux and very nice on your router, you can even install Unbound DNS on windows https://nlnetlabs.nl/projects/unbound/download/ 

Why is blocking hosts useful:
-----------------------------
If you want to make life easier for yourself, and/or your adblocker on your browser, don't connect to an advertising/illicit website at all in the first place, and then there's 1 less thing to block/cover up after the fact. The only possible downside is if you block something you find out you needed, but you're free to prune your list as you see fit. 
All this works by stopping your PC asking a DNS server for a website's IP address if you don't want it to load. Unbound DNS is special because it does more (and more securely) than regular DNS servers and lets your PC be selfsufficient without having to rely on any other DNS servers that may be actively recording your habits.

Backup a sec...what is a DNS server?
------------------------------------
A DNS server is a program that keeps a record of what a website's IP address is (likely to change quite regularly), to let you type in "www.youtube.com" (nice and readable) and not have to remember a bunch of numbers and have to navigate to 172.217.19.206 (ipv4)  OR  2a00:1450:400e:808::200e  (ipv6). These will still work of course! 

By default, your ISP gives you theirs to use, but some people use Cloudflare's (1.1.1.1) or Google's (8.8.8.8) as they can be faster, though really you're simply shifting the control of your browsing history to a new master.

When you're loading a web page (simplified)
Your PC asks DNS server for website's address, if it finds it, it tells you.


So how does this blocking work precisely?
-----------------------------------------
If you add entries to your 'hosts' file (C\Windows\system32\drivers\etc\hosts or /etc/hosts) or Unbound blocklist, they get looked at before asking out for them. In this way, you intercept the request by looking for the website's address on your own system, which stops any unwanted sites from being queried, so there's nothing to load, nothing to later block and your bandwidth can be used how YOU want it to be.

Unbound DNS server can block access to looking up a domain's IP address in 3 ways so far as I've discovered:

1) Refusing to look it up if asked       "always_refuse"
2) Saying the domain doesn't exist       "always_nxdomain"  
3) Directing it to 0.0.0.0 (as an alias)   "X redirect; X A 0.0.0.0"

Some research suggests option 2 may be faster (as it doesn't involve redirection?), it simply says "Nope. That's not real." rather than "I'm not letting you" or "sure it's....0.0.0.0"Tell me more about blocklists


So which blocklist or hosts file should I choose?
-----------------------------
By far the finest compilation of sites to block with a hosts file can be found at StevenBlack's here on github (https://github.com/StevenBlack/hosts/releases), sourced from multiple places including the classic "MVPS" etc
Typically, we neuter the connection by redirecting to 0.0.0.0 (some people use 127.0.0.1 but 0.0.0.0 is the foolproof/slightly faster choice.)

These "0.0.0.0 website.com" examples that work under dnsmasq etc need to be slightly tweaked for use with Unbound.


Final word on the files:
------------------------
They're config files which you're entirely free to copy and tweak to suit you. They helped me and I'm sharing them to help you. :) Especially if more people discover Unbound cause it's fantastic software.

I add a lot of my own personal entries to this file which I've stumbled across as I use the internet. Some might be a bit strict: In which case I suggest basing off a smaller set from StevenBlack's. or plucking out stuff. (A lot of social media stuff for example).

The very nice thing about Unbound is that the entries work recursively so far as I can tell. If you want to block:

b.madeupsite.com  you can put an entry for that. However, if you also want to block
a.b.madeupsite.com    the entry for b.madeupsite, because structurally it is queried BEFORE looking for the a site beneath it, blocks anything to follow and makes this entry redundant. In this way, you can write very stringent rules or very specific ones.

(i.e. blocking:

a.b.website.com               ONLY a.b.website.com is blocked
b.website.com                 both b.website.com and a.b.website.com are blocked
website.com                   a.website.com, a.b.website.com, AND website.com are all blocked

which means the blocklist can actually get significantly shortened (EDITING TO DO!)


Happy bandwidth reclaiming!
-Ant








