---
title: VCAP-DCV Deploy Exam Experience
author: Maarten Van Driessen
type: post
date: 2020-08-23T06:00:54+00:00
url: /vcap-dcv-deploy-exam-experience-2/
categories:
  - Certification
  - VMware
tags:
  - career
  - certification
  - vcap
  - VMware

---
After postponing this exam for too long, I finally took the time to study for and take the VCAP-DCV deploy exam. To be honest, I was kind of looking forward to this exam. I love playing in the lab and getting my hands dirty in an environment. So to be able to take an actual lab exam was pretty exciting to me.

In this post, I will try to give some pointers and tips that might help you pass the exam. Please note that I took the exam for the 6.5 version but the same logic applies to other versions.

## Preparation

Unlike the design exam, the deploy exam is a lab exam. This means you need to know how to do the stuff you've been reading and talking about. But, there are still some similarities. For starters, know the blueprint. This comes back for every exam, but read the blueprint upfront and know what topics are going to be covered on the exam. You should be intimately familiar with these blueprints by now.

### Documentation

Although it's a lab exam, you do have access to all the documentation VMware has on vSphere 6.5 in a folder on your desktop. Unlike what I read in some other posts, you do **NOT** have access to Adobe Reader in the lab. All PDFs will be opened in the browser, which means you can't do a search in the entire folder anymore. Keep this in mind, read the documentation, and know where you need to look to find certain commands or info on a topic.

### Community resources

As you might have guessed, there are already some great resources available in the community and I'm probably going to be linking the same ones that everyone does but that just shows how good they are.

  * [Kyle Jenner's VCAP6-DCV deployment study guide](http://www.vjenner.com/vcap6-dcv-deployment-study-guide/) is the resource that I used the most. Kyle has put a lot of time and effort into explaining every topic covered in the blueprint. Make sure to go through every topic thoroughly.
  * [Graham Barker's VCAP6-DCV exam simulator](https://virtualg.uk/vcap6-dcv-deploy-exam-simulator-free/) is great to get a feel for what kind of questions will be asked and what depth they go to. Although the HoLs listed are no longer available, you can still perform these tasks in another HoL or your own lab.

### Lab

But the most important resource I used while preparing was my lab. The VCAP exam covers nearly every vSphere feature there is. Like me, you're probably not familiar with every feature there is. Make sure you lab these things more than once so you know how to do them. If you've got a lab that's set up perfectly, try to get one of your colleagues or friends to break things in the lab. This will come in handy when you're doing the exam. If you don't have a lab, just spin up one of the vSphere 6.7 labs and start playing around.

You could also rent a server for a month or 2 like I did, and start building a nested lab. But that's a topic for another post.

Besides that, there's no replacement for real-world experience. I would not recommend taking this exam until you've got about 1 - 2 years of daily hands-on work done with vSphere. This will make the exam a lot easier.

## Exam experience

The exam itself is presented to you in a HoL like fashion, it's the same UI. If you're not familiar with how HoLs work, be sure to start up a few so you're familiar with the interface and how you can change and use the interface. Also, try booking the exam in a center where they have big screens. My test center has 24" screens, which helped A LOT.

I found the lab to be reasonably performant and had no issues with connection whatsoever. The only minor annoyance was letters appearing more than once while typing but this is probably due to latency. Just be sure to read what you typed if a command fails.

Time management is crucial for this exam, that's what a lot of other people told me at least. With that in mind, I tried to get the questions done as fast as I could without rushing through them. If I was stumped on one part of the question, I would write it down on the piece of paper I got, and move on to the next question. After I got through all questions, I started going back to the ones I didn't complete. This will also prevent you from getting frustrated/stuck on 1 question, taking a break will give you a fresh look at the question.

I finished the exam with 42 minutes to spare so I never really felt that I was in a hurry to get everything done but your mileage may vary.

When doing the exam, read the questions carefully. It happened several times that I quickly read a question, started doing things, and afterward re-reading the question to find out I had not done several things.

Make sure you're familiar with the CLI and PowerCLI, these things can come in handy for doing certain things faster. Also, try to open up the flash client again before taking the 6.5 exam. During the 6.5 days, the H5 client wasn't yet fully-featured so you may not be able to use it for all questions.

## Results

I took the exam on a Friday at 10 AM, so I was expecting to get the results on Monday or Tuesday after that. Around 8 PM I got an e-mail saying that I passed! This was a very pleasant surprise and big kudos to the VMware education team for providing the results so quickly.

I hope this post will help you prepare for the exam, good luck!