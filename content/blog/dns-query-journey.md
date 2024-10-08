+++
title = 'How a URL Becomes a Website'
date = 2024-09-22T13:54:02-05:00
+++

# Introduction

While studying for the AWS Certified Solutions Architect Associate Exam, I found myself diving more and more into networking. Before this, I never really thought about how the internet actually works. You type a URL, hit enter, and boom — you’re on the website. Seems like magic, right? But as it turns out, there’s a lot more going on behind the scenes. One of the key technologies making this happen is called DNS. Let’s break it down and see what’s really going while browsing to your favorite sites.

# What's in a URL

Every website on the internet lives somewhere on a server, and every server has a public IP Address attached to it. When we visit a website, we are actually visiting the IP Address of that server. We can use a Linux utility called `dig` (short for Domain Information Groper) to discover the IP Addresses of some of our favorite websites.
Using Linux or WSL on Windows, open the command line and enter:

```bash
dig www.google.com
```

You'll be returned with a list of results:

```
; <<>> DiG 9.18.28-0ubuntu0.22.04.1-Ubuntu <<>> www.google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 27361
;; flags: qr rd ra; QUERY: 1, ANSWER: 6, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 0
;; QUESTION SECTION:
;www.google.com.                        IN      A

;; ANSWER SECTION:
www.google.com.         155     IN      A       142.251.116.103
www.google.com.         155     IN      A       142.251.116.99
www.google.com.         155     IN      A       142.251.116.147
www.google.com.         155     IN      A       142.251.116.104
www.google.com.         155     IN      A       142.251.116.105
www.google.com.         155     IN      A       142.251.116.106
```

In the answer section, you can see different IP Addresses associated with Google. Choose any one of these and enter it into your browser's address bar, and you'll be directed to Google.

When we connect to a website, what we actually need is the IP Address of the server. Humans, however, are notoriously bad at remembering numbers, so we had to come up with a better way.

# Domain Names to the Rescue

To help facilitate our bad memory of numbers, we devised a way to represent IP Addresses with human-friendly names called domain names. You can easily remember `www.google.com`, but you probably already forgot the IP Address you connected to just a few seconds ago. This translation from human-friendly names to IP Addresses is called the Domain Name System (DNS for short).

At a basic level, DNS is a system of servers that accept requests for domain names (like `www.google.com`) and return the corresponding IP Address. Your computer then connects to that IP Address to visit the website.
![dnsquery-basic](/portfolio-site/dnsquery-basic.png)

A real-life example to help understand this is going to a friend's house. Your friend may say, "We're going to meet at Harry's house," but that doesn't actually tell you how to get there. You have to ask, "Okay, what's the address?" to find out that Harry lives at 4 Privet Drive. Now you can get to Harry's house.

# A Journey Through the Internet

In reality, DNS has a much more complex architecture. When you consider that every single website in the world needs domain-to-IP Address translation, you realize we have far too much data to store in a single DNS database. Additionally, every single user in the world needs to query to have domain names resolved to IP addresses, so we have a massive load problem too.

Instead, DNS had to be designed for scale. The way this is accomplished is by having a series of queries your computer goes through, where each query gets you one step closer to finding the IP Address you need. To help solidify this concept in my own mind, I drew out a diagram. Let's walk through it together.

![dnsquery](/portfolio-site/dnsquery.png)

We start off just like before: You type a domain name into your browser's address bar. We've made an optimization here though - before doing any queries, we first check our local DNS cache. By caching DNS results for websites recently visited, we can completely bypass making queries, thereby reducing load on the DNS servers and also speeding up our browsing. If the result is found in our cache, it returns the IP address and we're done. However, if it's not found, we will next query our DNS server.

Your DNS server's IP Address is pre-configured into your router. Many DNS servers exist, but unless you've manually configured it via your router, you're likely using the DNS server of your Internet Service Provider (ISP). The DNS server also contains a cache, and it starts by checking this. Since many people use the same DNS server as you, if someone has recently queried for the same website, the DNS server can return the IP address from the cache and avoid making multiple queries. If your DNS server has the data in its cache, your journey ends here.

Let's assume it doesn't though — this is where things get fun. To find out where to go next, your DNS server will query a `root nameserver`. Only 13 root nameservers exist in the world, and your DNS server knows how to query them — they are the starting point of every DNS query. Root servers don't contain the IP addresses for websites themselves, but act as a gateway by directing your DNS server to more specialized servers that handle specific top-level domains (TLDs), like `.com`, `.net`, or `.gov`.

When your DNS server queries the root nameserver, it will inspect the address you're trying to resolve and, based on the TLD, direct you to the server that contains information on that TLD. For example, if you're trying to access a `.com` address, the root nameserver will return the IP address of the `.com` nameserver.

Your DNS server now queries the `.com` nameserver, which inspects the address you're trying to resolve and returns the address of the next nameserver, getting you one step closer to the IP address you need. In my diagram, I am hosting my domain using Amazon's Route 53, so the `.com` nameserver will return the IP address for Amazon's Route 53 nameserver.

Now your DNS server queries Amazon's Route 53 nameserver, which contains the address of the website you're trying to visit. The Route 53 nameserver returns the IP address you've been looking for. Your DNS server caches that result and passes it back to your router, which also caches the result. Finally, your browser navigates to the IP address, and you're on the website.

The next time you or someone else queries the same domain, this entire process is skipped thanks to caching, allowing the browser to quickly navigate to the correct IP address.

# Conclusion

So now you have a better understanding of DNS - the key technology powering the internet. From translating easy-to-remember domain names into server IP addresses to caching results for faster browsing, it’s doing a lot more than we realize. Next time you type a URL, you’ll know just how much goes on behind the scenes to get you where you want to go.
