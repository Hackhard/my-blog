---
layout: post
title:  "Findings to ponder"
date:   2021-05-25 02:09:09 +0530
categories: jekyll update
---


Well as my Community Bonding has started, I've been asked by my mentor to break the problems into smaller ones and start creating issues as one could track and get the progress of the project through these.
If you haven't visited the [wiki](https://gitlab.torproject.org/woswos/CAPTCHA-Monitor/-/wikis/GSoC-2021) page of my project, please visit it to get the latest details of the project.

That said, my first aim was to compile the Captcha Monitor code locally to experiment with it but due to some errors couldn't compile it, but thanks to my mentor I did understand the basic architecture of the code.
Further he asked me to start my experiments on the smaller scripts that would soon be the base on which the new code would start developing.

So,

While writing the project I tested my trials using `request` library, and it has this pretty cool `status_code` method that returns the status_code of the website queried, and for the 
Google Search example returns `429 No Reason Phrase` in Tor but `200 Ok` in non-tor. 

Can be seen here: 

![image](https://user-images.githubusercontent.com/34208125/119404370-52fd1e80-bcfd-11eb-94dc-8d4433d716ce.png)

Now I'm using the Selenium Wire because it works on top of Selenium and provides some more powerful features (Selenium doesn't support status code) to Request Library as a whole.
So now I've Selenium-Wire returning me the entire dump of request status codes (Details like: What are the status code for the images in th website). 
At first I began thinking what if a request to a component is made in different order so I used `dict` to check if the `request path` is present and further check the difference in the `status codes`. 
But after running it I realized about the blunder I made, _what if just a particular image is not loaded?_ I see the above failing in this case and also realized the first request a client makes to a website is to it's root and further to the different components. The real life example I faced was with [Netflix](https://www.netflix.com/). 

The `'/scripttemplates/otSDKStub.js'` file appears to have two different status codes from two different browser: `403` from Tor and `200` from Non-tor. So this shows that this technique isn't reliable.

So now I'm just checking for the very first `status code` and checking for just the error codes, like 4xx and 5xx and not all, as we don't want websites reloading to other pages (GDPR, consent pages, website based on geo-ip) to label as Blocking-Tor.
Also I faced below issues:
+ With `http://` website as I was using `https://`. I guess I'll have to set Untrusted/Insecure Certificates.
+ The script I'm using [this code](https://gitlab.torproject.org/woswos/CAPTCHA-Monitor/-/snippets/60) tries to get status codes till all components aren't fetched. For websites like reddit, where it keeps loading slowly, it throws `Timeout loading page after 300000ms`. So I plan to break from the loop before the errror is reached.

That said for the website to be sorted using status code my final Logic would be :

- [ ] Check request[0] and if Tor == 4xx :
  - break (Blocked website)
- [ ] Check request[0] and if Tor == 3xx :
  - Check for redirection (Either could be Captcha redirection or Safe redirection or GDPR consent)
    - [ ] Click all possible Translation of "Accept", "Ok" etc.
    - Use of DOM tools and Consensus Module.
     
Any suggestions are welcomed.

Thanks!!


