---
layout: post
title:  "My Approach"
date: 2021-03-28 22:49:09 +0530
categories: jekyll update
---

Hi everybody,

After a bit of trial and errors on certain websites known to block the exit relays of Tor(reddit) found some interesting stuffs, that I didn't know of.

Websites like [Reddit (1)](https://reddit.com) or [Samsung](https://www.samsung.com/) shows error like: _"503 Service Temporarily Unavailable"_ or _"Access Denied"_ which concludes that these websites kind of blocks Tor fully.  You could see [the comment](https://gitlab.torproject.org/tpo/community/support/-/issues/40013#note_2728858). Basically it lists my findings about reddit, it shows that one can access https://reddit.com using Tor, by logging in, tho one cannot remain annonymous after logging in, as the website knows the profile and get's quite some information, and that doesn't remain anonymous browsing anymore.
Though there are alternatives like: [old reddit](https://old.reddit.com) and [reddit ssl](https://ssl.reddit.com) that doesn't require logging in and one can browse annonymously.
But other websites like [instagram](https://instagram.com) one needs to login in, to view the content. 

Websites like [Google (2)](https://google.com) loads without any restrictions, but searching up something, like maybe "Tor" [Google Search Tor](https://www.google.com/search?q=tor) returns Captcha. 
Refer the image below: 

![image](https://user-images.githubusercontent.com/34208125/112763802-a582cd00-9023-11eb-87bb-073797a82795.png)

There is also a specific case where the websites like: [Lloyds Bank (3)](https://www.lloydsbank.com/) and [Papa John's Pizza](https://www.papajohns.com/) which returns a website with `200 OK`status code, but the webpage generally displays : _"We are sorry an error has occurred, please try again later."_ . This shows us that these websites are partially blocked over Tor.

Last but not the least, websites such as [Github (4)](https://github.com/) that doesn't block Tor at all!

----------------------------------------------------------------------------------------

Above mentioned points can therefore be divided into 4 parts. They are:

1. Websites that don't block Tor      [4]
2. Websites that partially block Tor  [3]
3. Websites that returns captchas     [2]
4. Websites that completely block Tor [1]

# Next Steps: #

## The Overall Flowchart: ##

![image](https://user-images.githubusercontent.com/34208125/112788271-266bb400-9078-11eb-9a72-6932a6e7291d.png)

## Working: ##

The main focus is to track the websites including [**Alexa Top 500 sites**](https://www.alexa.com/topsites) blocking Tor exit nodes fully or partially. So, a basic approach would be comparing the Http status codes, Headers (browser dependant or not), DOM Tree of a given website with Tor exit node and non-Tor exit node, but it might not always give the correct results. So I’ll be trying to categorize the results into cases and try to cover them all. Below I’m trying to broaden each path:

The non-Tor path is sort of a role-model path. We’ll compare the Tor path to this to find if there are any differences between the two paths, more extensively the tor exit nodes and browsers over tor to that of the non tor browsers. Since we are going to compare the information, we will save all the information that might help us with the comparison part. We are first going to fetch the Http headers, the http status codes, to get information of the websites (superficial-information) that might help to differentiate between the Tb [1] and Nb [2] without the need of scanning the whole website. We might achieve results easily for cases when `status_code(Tb) != status_code(Nb)`: 

Image 1 | Image 2
--------|--------
![image](https://user-images.githubusercontent.com/34208125/112788793-451e7a80-9079-11eb-8ff3-812e4a942870.png) | ![image](https://user-images.githubusercontent.com/34208125/112788866-75661900-9079-11eb-928e-2083aac75f91.png)

Or even in cases like these: 





