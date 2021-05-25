---
layout: post
title:  "Findings to ponder"
date:   2021-05-25 02:09:09 +0530
categories: GSoC
---


Well as my Community Bonding period has started, I've been asked by my mentor to break the problems into smaller ones and start creating issues as one could track and get the progress of the project through these. If you haven't visited the [wiki](https://gitlab.torproject.org/woswos/CAPTCHA-Monitor/-/wikis/GSoC-2021) page of my project, please visit it to get the latest details of the project.



That said, my first aim was to compile the Captcha Monitor code locally to experiment with it but due to some errors couldn't compile it, but thanks to my mentor I did understand the basic architecture of the code. Further, he asked me to start my experiments on the smaller scripts that would soon be the base on which the new code would start developing.

So,

While writing the project I tested my trials using the `request` library, and it has this pretty cool `status_code` method that returns the status_code of the website queried, and for the 
Google Search example returns `429 No Reason-Phrase` in Tor but `200 Ok` in non-tor. 

Can be seen here: 

![image](https://user-images.githubusercontent.com/34208125/119404370-52fd1e80-bcfd-11eb-94dc-8d4433d716ce.png)


Now I'm using the Selenium Wire because it works on top of Selenium and provides some more powerful features (Selenium doesn't support status code) to Request Library as a whole.
So now I've Selenium-Wire returning me the entire dump of request status codes (Details like What are the status code for the different components including images of a particular website). 
At first, I began thinking what if a request to a component is made in a different order so I used `dict` to check if the `request path` is present and further check the difference in the `status codes`. 
But after running it I realized about the blunder I made, _what if just a particular image is not loaded?_ I see the above failing in this case and also realized the first request a client makes to a website is to its root and further to the different components. The real-life example I faced was with [Netflix](https://www.netflix.com/). 

The `'/scripttemplates/otSDKStub.js'` file appears to have two different status codes from two different browsers: `403` from Tor and `200` from Non-tor. So this shows that this technique isn't reliable.

Now I plan to check for the very first `status code` and then check for just the error codes, like 4xx and 5xx and not all ( bad request (a 4XX client error or 5XX server error response)`raise_for_status()`), as we don't want websites redirecting to other pages (GDPR, consent pages, website based on geo-ip) to be labeled as Blocking-Tor.

That being said the issues I faced were :
+ With `http://` website as I was using `https://`. 
Citing an example below, while I was going through this particular [website](http://adsabs.harvard.edu)  :

Client | http://adsabs.harvard.edu  | https://adsabs.harvard.edu
------| ------- | -------------
Non-Tor | ![non-tor_ http:adsabs harvard edu](https://user-images.githubusercontent.com/34208125/119474622-fb48cc80-bd69-11eb-943a-2d94459037d5.png) | ![non-tor_ https:adsabs harvard edu](https://user-images.githubusercontent.com/34208125/119474744-1fa4a900-bd6a-11eb-9bae-2b18d7f5248a.png)
Tor | ![tor_ http:adsabs harvard edu](https://user-images.githubusercontent.com/34208125/119474807-2df2c500-bd6a-11eb-87fb-9cd3424331d1.png) | ![tor_ https:adsabs harvard edu](https://user-images.githubusercontent.com/34208125/119474852-35b26980-bd6a-11eb-9b75-eebc74b77892.png)

Clicking the above links, you'll notice that the browser too loads the http version. I tried setting Untrusted/Insecure Certificates to true but didn't work.

+ The script I'm using [this code](https://gitlab.torproject.org/woswos/CAPTCHA-Monitor/-/snippets/60) tries to get status codes till all components aren't fetched. For websites like Reddit, where it keeps loading slowly, it throws `Timeout loading page after 300000ms`. So I plan to break from the loop before the error is reached or a try catch block to proceed ahead.

Hence, for the website to be checked using status code my final Logic would be :

- [ ] Check `request[0]` and if ```Tor == 4xx``` or `Tor == 5xx` :
  - break (Blocked website)
- [ ] Check `request[0]` and if `Tor == 3xx ` :
  - Check for redirection (Either could be Captcha redirection or Safe redirection or GDPR consent)
    - [ ] Click all possible Translation of "Accept", "Ok" etc.
    - Use of DOM tools and Consensus Module.
     
Any suggestions are welcomed.

Thanks!!

----------

Edit:

The `HTTP(S)` error I faced was because I was forcing `https` request on websites supporting `http` requests and thereby returning the error shown by the image. 
I wasn't using `http` by deafult because my ISP randomly injects Advertisement on `http` websites (DNS Poisoning). I had been using Cloudflare DNS from before but seems like it isn't working in my case. So today I queried in irc and got answers like using own recursors or DNS-over-TLS/HTTPS (DoT/DoH) but I feel my ISP intercepts my dns traffic too (That opens up another topic for research). I got across NextDNS (DoT with Blacklist) and it seems to be working as of now :)

Meanwhile grepping around my folders containing seperate folders for each website (for easy debugging), I noticed `captcha` appearing in my reqeust_paths. Might be of help to distinguish a website from returning Captchas or not.


