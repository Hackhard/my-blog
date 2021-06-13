---
layout: post
title:  "Coding Phase Begins"
date:   2021-03-27 02:09:09 +0530
categories: jekyll update
---

After 2 PRs merged. I started writing the `Analyser.py` but was not able to get the idea of writing the test cases and also visualize my approached codes. Adding to my concerns, the ssh webserver I had lent off started having high IOPS and hence breaking in. Adding to it my tor too kept stucking at `Starting with guard context "default"`. Thanks to my mentor who advised me to complete the `analyser.py` and then think about adding it into the Project.

![image](https://user-images.githubusercontent.com/34208125/121799926-a1427500-cc4c-11eb-83d6-b6b7de3ed411.png). I tried contacting the members but seemed my internet wasn't allowing Tor which proved not to be True as I checked with the [Emma](https://gitlab.torproject.org/tpo/anti-censorship/emma) and OONI probe and saw my connections were fine if not good, as my IPV6 wasn't working. Next I tried tinkering and thought of `purging` and `installing` tor again. To my wonder it did work! 

Back from the ssh (Not totally) but for the time-being as my home network is quite slow. Sadly I keep ranting about all of these :( because I'm a bit frustrated about the technical errors. 

Anyway let's deep dive what I did this week:

## Progress:

I updated my flow of work to the below flowchart and still have few points to add.

[![](https://mermaid.ink/img/eyJjb2RlIjoiJSUgRW5hYmxlIEpTIHRvIHNlZSB0aGlzIFxuJSUgVXNlIG9mIGRhc2hlZCBsaW5lcyBhbmQgYm94ZXMgc2hvdyB0aGUgdGhpbmdzIHRoYXQgaGF2ZW4ndCBiZWVuIGltcGxlbWVudGVkIGFzIG9mIG5vdy4gXG5ncmFwaCBURFxuc3ViZ3JhcGggRmV0Y2hcbkFbRmV0Y2ggdXJsIHVzaW5nIFRvciBjbGllbnRdXG5CW0ZldGNoIHVybCB1c2luZyBOb24tVG9yIGNsaWVudF1cbmVuZFxuc3ViZ3JhcGggRE9NIEFuYWx5c2lzXG5FO0UxO0UyO0c7RzE7RzI7SDtIMjtYXG5lbmRcbkFbRmV0Y2ggdXJsIHVzaW5nIFRvciBjbGllbnRdIC0tPiBDe0lzIHRoZSA8YnI-IHN0YXR1cyBjb2RlIDxicj4gc2FtZT99LS1ZRVM6IDxicj4gTm8gcmVkaXJlY3Rpb24sIERPTSBjaGVja3MgcmVxdWlyZWQtLT5EMVtSZW1vdmUgR0RQUiBwb3B1cHNdXG5CW0ZldGNoIHVybCB1c2luZyBOb24tVG9yIGNsaWVudF0gLS0-Qy0tTk8tLS0-RHtjaGVjayB3aGV0aGVyIDxicj4gdG9yIHJldHVybnMgNHh4IG9yIDV4eCA8YnI-ZXJyb3IgY29kZXN9LS1ZRVMtLS0tLS0tLS0tPkYoVG9yIEJsb2NrIEVycm9yKVxuRC0tTk86IDxicj4gQ291bGQgYmUgR0RQUiwgcmVkaXJlY3Rpb24sIENhcHRjaGEtLT5FNC0uTm8tLT5EMS0tPkVbL0FkZGl0aW9uYWwgVGVzdHMgPGJyPiBET00gY2hlY2tzL11cbkUtLT5FMVtET00gQ2hlY2tzIDxicj4gUGVyY2VudGFnZSBvZiBkaWZmZXJuY2UgaW4gRE9NIG5vZGVzXVxuRS0uLT5FMltDb25zZW5zdXMgTW9kdWxlXVxuRS0tLS0tLT5FM1tDYXB0Y2hhIENoZWNrXSAlJSBUaGlzIHdpbGwgdXNlIHRoZSBmYWN0IG9mIHRoZSByZXF1ZXN0IHBhdGggY29udGFpbmluZyBjYXB0Y2hhIGluIHRoZSB1cmwgaXRzZWxmXG5FNHtJZiB0aGUgPGJyPiBSZWRpcmVjdGVkIHdlYnNpdGUgPGJyPnJldHVybnMgZXJyb3J9LS15ZXMuLT5GXG5FMS0tPkd7aWYgPGJyPiBzY29yZSA-IDB9XG5HLS1OTywgaWYgc2NvcmUgPSAwLS0tPlgoTWF0Y2hlZCE8YnI-bm8gZXJyb3JzKVxuRy0tWUVTLS0-RzF7aWYgPGJyPiBzY29yZSA-IEslfS0tWUVTLS0tLT5IMihUb3IgcmV0dXJucyBFcnJvciA8YnI-IG9yIGluIHNvbWUgY2FzZXMsIGRlbm90ZXMgYSBkaWZmZXJlbnQgcGFnZSkgJSUgRm9yIG1vc3QgY2FzZXMgaXQgcmV0dXJucyBlcnJvciBvciBpdCBtaWdodCBiZSBwb3NzaWJsZSB0aGF0IHRoZSBwYWdlIGhhc24ndCBiZWVuIGxvYWRlZC5cbkcxLS1OTy0tPkcyKEZpbHRlciBsaXN0KVxuRzEtLk5vLS4uLT5FMlxuRy0tTk8tLS0tLT5IW0Rlbm90ZXMgUG9wLXVwcywgPGJyPm9yIGluIHNvbWUgY2FzZXMgPGJyPndoZW4gdGhlIGRpZmZlcmVuY2UgaXMgbG90IGluIG5lZ2F0aXZlIHRlcm1zLSB0YmIgPiBuYmI8YnI-ZGVub3RlcyBhbm90aGVyIHBhZ2UsIHdoaWNoIG1pZ2h0IGhhdmUgbW9yZSBET00gbm9kZXMuXVxuQS0tLS0tLS0tLS0tLS0-UShXZWJzaXRlcyB3aXRob3V0IGVycm9yLCBidXQgZGlmZmVyZW50IHBhZ2VzKVxuXG5cblxuY2xpY2sgUSBocmVmIFwiaHR0cDovL3d3dy5kb21pbm9zLmNvbVwiIF9ibGFua1xuJSUgc3R5bGUgRDEgc3Ryb2tlLXdpZHRoOjFweCxzdHJva2UtZGFzaGFycmF5OiA1IDhcbiUlIHN0eWxlIEU0IHN0cm9rZS13aWR0aDoxcHgsc3Ryb2tlLWRhc2hhcnJheTogNSA4XG4lJSBzdHlsZSBFMyBzdHJva2Utd2lkdGg6MXB4LHN0cm9rZS1kYXNoYXJyYXk6IDUgOFxuc3R5bGUgRTIgc3Ryb2tlLXdpZHRoOjFweCxzdHJva2UtZGFzaGFycmF5OiA1IDhcbnN0eWxlIEYgc3Ryb2tlLXdpZHRoOjNweCxmaWxsOiNmMDQiLCJtZXJtYWlkIjp7InRoZW1lIjoibmV1dHJhbCJ9LCJ1cGRhdGVFZGl0b3IiOmZhbHNlLCJhdXRvU3luYyI6dHJ1ZSwidXBkYXRlRGlhZ3JhbSI6ZmFsc2V9)](https://mermaid-js.github.io/mermaid-live-editor/edit##eyJjb2RlIjoiJSUgRW5hYmxlIEpTIHRvIHNlZSB0aGlzIFxuJSUgVXNlIG9mIGRhc2hlZCBsaW5lcyBhbmQgYm94ZXMgc2hvdyB0aGUgdGhpbmdzIHRoYXQgaGF2ZW4ndCBiZWVuIGltcGxlbWVudGVkIGFzIG9mIG5vdy4gXG5ncmFwaCBURFxuc3ViZ3JhcGggRmV0Y2hcbkFbRmV0Y2ggdXJsIHVzaW5nIFRvciBjbGllbnRdXG5CW0ZldGNoIHVybCB1c2luZyBOb24tVG9yIGNsaWVudF1cbmVuZFxuc3ViZ3JhcGggRE9NIEFuYWx5c2lzXG5FO0UxO0UyO0c7RzE7RzI7SDtIMjtYXG5lbmRcbkFbRmV0Y2ggdXJsIHVzaW5nIFRvciBjbGllbnRdIC0tPiBDe0lzIHRoZSA8YnI-IHN0YXR1cyBjb2RlIDxicj4gc2FtZT99LS1ZRVM6IDxicj4gTm8gcmVkaXJlY3Rpb24sIERPTSBjaGVja3MgcmVxdWlyZWQtLT5EMVtSZW1vdmUgR0RQUiBwb3B1cHNdXG5CW0ZldGNoIHVybCB1c2luZyBOb24tVG9yIGNsaWVudF0gLS0-Qy0tTk8tLS0-RHtjaGVjayB3aGV0aGVyIDxicj4gdG9yIHJldHVybnMgNHh4IG9yIDV4eCA8YnI-ZXJyb3IgY29kZXN9LS1ZRVMtLS0tLS0tLS0tPkYoVG9yIEJsb2NrIEVycm9yKVxuRC0tTk86IDxicj4gQ291bGQgYmUgR0RQUiwgcmVkaXJlY3Rpb24sIENhcHRjaGEtLT5FNC0uTm8tLT5EMS0tPkVbL0FkZGl0aW9uYWwgVGVzdHMgPGJyPiBET00gY2hlY2tzL11cbkUtLT5FMVtET00gQ2hlY2tzIDxicj4gUGVyY2VudGFnZSBvZiBkaWZmZXJuY2UgaW4gRE9NIG5vZGVzXVxuRS0uLT5FMltDb25zZW5zdXMgTW9kdWxlXVxuRS0tLS0tLT5FM1tDYXB0Y2hhIENoZWNrXSAlJSBUaGlzIHdpbGwgdXNlIHRoZSBmYWN0IG9mIHRoZSByZXF1ZXN0IHBhdGggY29udGFpbmluZyBjYXB0Y2hhIGluIHRoZSB1cmwgaXRzZWxmXG5FNHtJZiB0aGUgPGJyPiBSZWRpcmVjdGVkIHdlYnNpdGUgPGJyPnJldHVybnMgZXJyb3J9LS55ZXMuLT5GXG5FMS0tPkd7aWYgPGJyPiBzY29yZSA-IDB9XG5HLS1OTywgaWYgc2NvcmUgPSAwLS0tPlgoTWF0Y2hlZCE8YnI-bm8gZXJyb3JzKVxuRy0tWUVTLS0-RzF7aWYgPGJyPiBzY29yZSA-IEslfS0tWUVTLS0tLT5IMihUb3IgcmV0dXJucyBFcnJvciA8YnI-IG9yIGluIHNvbWUgY2FzZXMsIGRlbm90ZXMgYSBkaWZmZXJlbnQgcGFnZSkgJSUgRm9yIG1vc3QgY2FzZXMgaXQgcmV0dXJucyBlcnJvciBvciBpdCBtaWdodCBiZSBwb3NzaWJsZSB0aGF0IHRoZSBwYWdlIGhhc24ndCBiZWVuIGxvYWRlZC5cbkcxLS1OTy0tPkcyKEZpbHRlciBsaXN0KVxuRzEtLk5vLS4uLT5FMlxuRy0tTk8tLS0tLT5IW0Rlbm90ZXMgUG9wLXVwcywgPGJyPm9yIGluIHNvbWUgY2FzZXMgPGJyPndoZW4gdGhlIGRpZmZlcmVuY2UgaXMgbG90IGluIG5lZ2F0aXZlIHRlcm1zLSB0YmIgPiBuYmI8YnI-ZGVub3RlcyBhbm90aGVyIHBhZ2UsIHdoaWNoIG1pZ2h0IGhhdmUgbW9yZSBET00gbm9kZXMuXVxuQS0tLS0tLS0tLS0tLS0-UShXZWJzaXRlcyB3aXRob3V0IGVycm9yLCBidXQgZGlmZmVyZW50IHBhZ2VzKVxuXG5cblxuY2xpY2sgUSBocmVmIFwiaHR0cDovL3d3dy5kb21pbm9zLmNvbVwiIF9ibGFua1xuJSUgc3R5bGUgRDEgc3Ryb2tlLXdpZHRoOjFweCxzdHJva2UtZGFzaGFycmF5OiA1IDhcbiUlIHN0eWxlIEU0IHN0cm9rZS13aWR0aDoxcHgsc3Ryb2tlLWRhc2hhcnJheTogNSA4XG4lJSBzdHlsZSBFMyBzdHJva2Utd2lkdGg6MXB4LHN0cm9rZS1kYXNoYXJyYXk6IDUgOFxuc3R5bGUgRTIgc3Ryb2tlLXdpZHRoOjFweCxzdHJva2UtZGFzaGFycmF5OiA1IDhcbnN0eWxlIEYgc3Ryb2tlLXdpZHRoOjNweCxmaWxsOiNmMDQiLCJtZXJtYWlkIjoie1xuICBcInRoZW1lXCI6IFwibmV1dHJhbFwiXG59IiwidXBkYXRlRWRpdG9yIjpmYWxzZSwiYXV0b1N5bmMiOnRydWUsInVwZGF0ZURpYWdyYW0iOmZhbHNlfQ)


+ Use of dashed lines and boxes show the things that haven't been implemented as of now.
+ As of now K has been set to 150 (Experimental Analysis)
+ The Captcha Checking Module has been proposed recently, which enables the use of "captcha" in the requests path from the responses we get while we load a website.

For more details one you could look into the [Experimental code](https://raw.githubusercontent.com/Hackhard/Fetcher/main/status%20code/test_run4/tr.py) and it's [output](https://raw.githubusercontent.com/Hackhard/Fetcher/main/status%20code/test_run4/tr_bash_output) to gain an even more insight.

## Insights:

At present I'm facing with the reliability of the modules, like for example:

```Cloudflare blocks requests library and hence request library isn't suited in here. I read it's because the headers(User-Agent) of the request is sent by the name of python which get's marked as a bot. I changed the User-Agent but it was still the same so it isn't much of use in this case. Also for specific cases like mastercard where there is status 3xx (reload) it returns results easily.```

![image](https://user-images.githubusercontent.com/34208125/121801062-0a2ceb80-cc53-11eb-9933-19df70791265.png)

So I plan on also making a checker method that would check the following:
```Python
def check():
# Non Tor:
  if request_module is blocked:
    # Just to be cautious
    check HAR
    if HAR returns 3xx or 4xx or 5xx:
      go with request
    else:
      go with HAR.first()
# Tor:
  if request_module is blocked:
    check HAR 
    if HAR returns 3xx or 4xx or 5xx:
      go with request
    else:
      go with HAR.first()
 ```
 I hope this would tend to make the code a bit better in terms of reliabilty.
 Also HAR.first() mean the first request status code sent to server. Generally the index page.
    

