---
layout: post
title:  "My Approach"
date: 2021-03-29 13:20:09 +0530
categories: jekyll update
---

Hi everybody,

After a bit of trial and errors on certain websites known to block the exit relays of Tor(reddit) found some interesting stuffs, that I didn't know of.

Websites like [Reddit (i)](https://reddit.com) or [Samsung](https://www.samsung.com/) shows error like: _"503 Service Temporarily Unavailable"_ or _"Access Denied"_ which concludes that these websites kind of blocks Tor fully.  You could see [the comment](https://gitlab.torproject.org/tpo/community/support/-/issues/40013#note_2728858). Basically it lists my findings about reddit, it shows that one can access https://reddit.com using Tor, by logging in, tho one cannot remain anonymous after logging in, as the website knows the profile and gets quite some information, and that doesn't remain anonymous browsing anymore.
Though there are alternatives like: [old reddit](https://old.reddit.com) and [reddit ssl](https://ssl.reddit.com) that doesn't require logging in and one can browse anonymously.
But other websites like [Instagram](https://instagram.com) one needs to login in, to view the content. 

Websites like [Google (ii)](https://google.com) loads without any restrictions, but searching up something, like maybe "Tor" [Google Search Tor](https://www.google.com/search?q=tor) returns Captcha. 
Refer the image below: 

![image](https://user-images.githubusercontent.com/34208125/112763802-a582cd00-9023-11eb-87bb-073797a82795.png)

There is also a specific case where the websites like: [Lloyds Bank (iii)](https://www.lloydsbank.com/) and [Papa John's Pizza](https://www.papajohns.com/) which returns a website with `200 OK`status code, but the webpage generally displays : _"We are sorry an error has occurred, please try again later."_ . This shows us that these websites are partially blocked over Tor.

Last but not the least, websites such as [Github (iv)](https://github.com/) that doesn't block Tor at all!  


Above mentioned points can therefore be summarized into below 4 points. They are:

1. Websites that don't block Tor      [iv]
2. Websites that partially block Tor   [iii]
3. Websites that returns captchas     [ii]
4. Websites that completely block Tor [i]

### **Detailed Prospective:** ###

Below I've tried adding the low level flowchart, as well as the working and the logic part, that could be useful in the future.

#### The Overall Flowchart: ####

![image](https://user-images.githubusercontent.com/34208125/112788271-266bb400-9078-11eb-9a72-6932a6e7291d.png)

#### Working: ####

The main focus is to track the websites including [**Alexa Top 500 sites**](https://www.alexa.com/topsites) blocking Tor exit nodes fully or partially. So, a basic approach would be comparing the Http status codes, Headers (browser dependant or not), DOM Tree of a given website with Tor exit node and non-Tor exit node, but it might not always give the correct results. So I’ll be trying to categorize the results into cases and try to cover them all. Below I’m trying to broaden each path:

The non-Tor path is sort of a role-model path. We’ll compare the Tor path to this to find if there are any differences between the two paths, more extensively the tor exit nodes and browsers over tor to that of the non tor browsers. Since we are going to compare the information, we will save all the information that might help us with the comparison part. We are first going to fetch the Http headers, the http status codes, to get information of the websites (superficial-information) that might help to differentiate between the Tb [1] and Nb [2] without the need of scanning the whole website. We might achieve results easily for cases when `status_code(Tb) != status_code(Nb)`: 

Website Over Tor | Website not over Tor
--------|--------
![image](https://user-images.githubusercontent.com/34208125/112788793-451e7a80-9079-11eb-8ff3-812e4a942870.png) | ![image](https://user-images.githubusercontent.com/34208125/112788866-75661900-9079-11eb-928e-2083aac75f91.png)
{Fig 1.1}

Or even in cases like these: 

Website over Tor | Website not over Tor
--------|--------
![image](https://user-images.githubusercontent.com/34208125/112789352-882d1d80-907a-11eb-92ee-a012a2bc8bc6.png) | ![image](https://user-images.githubusercontent.com/34208125/112789391-9b3fed80-907a-11eb-90a6-88012df9c589.png)
{Fig 1.2}

If for some reasons we cannot differentiate using the superficial information. Let’s say that the Tb and Nb return the same status codes `status_code(Tb) == status_code(Nb)` and we cannot determine the differences. 
We next move to other options like:
* Compare the _length of generated DOM_ from both Tb and Nb, which might work for a case below: 
 
Website over Tor | Website not over Tor
--------|--------
![image](https://user-images.githubusercontent.com/34208125/112789546-f4a81c80-907a-11eb-8961-4563fb040236.png) | ![image](https://user-images.githubusercontent.com/34208125/112789554-f83ba380-907a-11eb-9192-f65e649e344d.png)
{Fig 1.3}

Here we can see that the status codes are `200`, but still there is a clear difference between the results, and for the same reason I’m planning to compare the length. 

* Now, for such cases where there might be almost same length, we again cannot surely determine if the results are different. So we may approach it preparing the `consensus` of each website. _We parse the DOM elements into a tree type structure with hashes, called `Senser` and collect the structure from all different NB we have (Chromium, Firefox, cURL, Requests) such that we get the picture of the unblocked-website we are looking for, and then accordingly get the results:_
 
	* If `Tb ≅  Nb` we know the Tor isn’t blocked.
  
	* If `Tb != Nb` we know that the specific Tor fingerprint is blocked.

#### The Logic: ####

Below I’ve tried attaching the high-level _Flow-chart_ that details about the process followed by the _pseudocode_. 

##### HIGH LEVEL FLOW-CHART: #####

![image](https://user-images.githubusercontent.com/34208125/112789927-d4c52880-907b-11eb-96da-706d7cd25ab9.png)

##### PSEUDO-CODE: #####

```Python3
Tb = TOR(url)   # fetch the url using Tor

Nb = BROWSER(url)   # fetch the url using normal browser

if (status_code(Tb) != status_code(Nb)): 
  return false      # either returns captcha or is blocked.
  
else:

  if (DOM_len(Tb) << DOM_len(Nb)):	# << denotes much larger than
    return false    # The Website might be partially blocked. e.g. Fig: 1.3
    
  else:
  
    p_c_t = prepare_consensus(Tb)   # Prepare consensus using Tor.
    p_c_n = prepare_consensus(Nb)   # Prepare consensus using normal browser.
    
    if (p_c_n == p_c_t):
    	return true     # The Website is similar
    else:
	return false    # The Website is not similar
  ```
  
  
#### **Further Works and Ideas:** ####

The ideas I will be aiming on are mentioned below, the basic idea for the Captcha Checker module has been discussed above.

* Working on the Senser paper (implementing a demo code to see how it works). 
* Working on Dockers or easy-local installation.
* Working on the metrics and graphs related to the Dashboard.
* Also further experimenting on few cloudflare sites or maybe registering a cloudflare website (to get some better idea on how cloudflare works for Tor)  

Refs: [website 1](https://blog.cloudflare.com/cloudflare-onion-service/), [website 2](https://blog.cloudflare.com/the-trouble-with-tor/), [website 3](https://blog.torproject.org/trouble-cloudflare?page=1)


-----------------------------------------
Tb [1] : Tor Browser  
Nb [2] : Normal Browser  
Senser paper could be accessed [here](https://giia.cs.georgetown.edu/~msherr/papers/senser.pdf)




