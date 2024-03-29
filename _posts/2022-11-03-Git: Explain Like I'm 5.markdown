---
layout: post
title:  "Git: Explain Like I'm 5"
date:   2022-11-03 02:41:09 +0700
categories: gtf
---



### Introduction

Git is a version control Swiss army knife.

![](https://upload.wikimedia.org/wikipedia/commons/1/10/Swiss_army_knife_open%2C_2012-%2801%29.jpg)
From: Wikimedia

> What is a Swiss army knife?

Well, a Swiss army knife is way of describing something that is super versatile and has a tool for most purposes and therefore is an art of great engineering.

> What is version control?

Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later.


For example, imagine that you have a computer that has nothing on it but a text editor and you decided to write a software program on this system. Since you’re a responsible software developer, you decide that you need to invent a method to keep track of versions of your software so that you can retrieve code that you previously changed or deleted (`changelogs`), or let's say if your code gets unnecessary errors you have a backup that you can always look up to (`revert`), instead of manually saving multiple copies.

This system that you wish you had while dealing with the problem is basically a VCS (Version Control System).

There are three basic kinds of VCS :
- Local
- Centralised
- Distributed

The gist being described by the adjectives:  
+ One being found locally
+ Another being used centrally over a particular server
+ And, the third being distributed along multiple servers for loss prevention.

_You can find more details in [here](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control)._

### How does it work and what's the benefit?

| ![](https://explainxkcd.com/wiki/images/4/4d/git.png) | 
|:--:| 
| *From: https://explainxkcd.com/* |

Suppose, you and your friend, let's name him "X", decide to draw a picture, you can sketch and your friend can paint, but due to covid you both want to work on it from each of your home. 

The teacher creates two photocopies of what you have started and you both take a copy home (`pulling/checking` out). You both add your creativity and genius to the drawing and when you come back to school a few days later, the teacher cuts and pastes the bits you `added / changed` and then photocopies that (`merging`). 

If for instance you both added a house to the picture in the same area, someone has to choose which portion to use (`resolving a conflict`). And since there are photocopies every time something is changed, you can throw away all the new ones if you don't like what you've done (`reverting`) and in this way you both can collaborate and work together while not being together and not depending on each other.

Thereby, increasing the individual productivity.

#### Some Resources:

- https://tom.preston-werner.com/2009/05/19/the-git-parable.html
- http://www-cs-students.stanford.edu/~blynn/gitmagic/book.html



