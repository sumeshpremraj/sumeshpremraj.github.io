---
title: "Directi interview experience"
date: "2014-01-26"
tags: ["interviews"]
---

Directi, one of the highest paying Indian companies(with a tough interview process to match) had a recruitment drive on our campus (MSRIT, Bangalore) for the post of operations engineer - which is a mix of coding, deployment, monitoring and troubleshooting. A few friends asked what preparation I did, questions asked in the interview etc, so I'm writing it down here instead of answering individually. I hope it will help you in preparing for interviews in general, or Directi in particular :)

I did some reading up online about the company, and found that it had multiple verticals like web hosting, domain registration, advertising, communication tools etc. Read a few employee reviews and saw that it was a growing company with its startup DNA mostly intact. This excited me because I talked to seniors in various companies and the consensus was that startups make you work hard but you also get to learn and grow fast. 

The hiring process had the following rounds - aptitude, online coding, and 3 or more rounds of interviews, each being an elimination round.

# Aptitude (30 mins)
Absolutely loved the fact that questions were all technical - a mix of Unix/Linux, networking, OS and C questions. Scoring was +4/-1 per question, so I answered only 18/30 questions I was sure of. This round was probably to pick out the technically competent candidates.

# Coding (60-90 mins)
We were asked to write an [nmap](http://en.wikipedia.org/wiki/Nmap) clone. They showed the nmap output for a local test server, and we had to replicate the functionality. Language could be any of C, Java, Python or Ruby (in line with the company's technology-agnostic philosophy). The official documentation for each language was uploaded to the local server and we were allowed to use only this for reference.

I read the C docs and wrote some code, but didn't get it working. I have been learning Python recently, and tried reading Python docs and soon found the functions needed for the job. Finished it in less than half hour (in addition to the half hour I spent attempting it in C).

Doing this round in Python caught the eye of the recruiter, which helped in later rounds :) He also appreciated how I used a built-in function to map the port number to service name in just one line instead of doing it manually.

### Interviews: Technical round #1
This round was mostly basic tech questions (that you can prepare from engineering texts) - OS, database, internet, Linux (including a discussion on Linux distros, file systems etc), networking. Answered most questions and admitted to not knowing the rest (instead of guessing and wasting time).

### Technical round #2
This round was mostly focused on internet technologies - types of web servers, databases, redundancy, high availability, scaling, DNS, data center designs, clustering etc.

There were many architectural decision questions: what RAID for high throughput high availability storage (RAID 10, RAID 4 or 5 depending on the use case), ideal RAID for a database, web application scaling techniques - using separate DB and app server, using multiple app servers, load balancers (Varnish), using Nginx or similar as a static file server to bypass the dynamic web server, switching entirely to Nginx or alternate servers, CDNs etc.

You have to be well versed in the practical aspects rather than focus on mugging up a few questions, as they could ask you any of a million questions from these above topics. Here's an anecdote: I didn't remember the full form of BIOS and RAID (blame the nerves), but explained the concepts well. That was enough for the interviewer.

In each round, I was asked about projects and extracurriculars. Saw my published WordPress and Ghost themes and was asked about that too, and then had a discussion about this new CMS called [Ghost](https://ghost.org/).

Tip: having code on github always helps.

### Interview #3 (managerial/HR)
I was honest about both my strengths and concerns. These interviewers must have seen hundreds of students give curated answers which feel completely artificial and made up, so I went the exact opposite route. Just saying "I am hardworking, passionate, intelligent" etc is not how you do it - you have to convince them with examples and anecdotes from your own life.

I talked about my <strong>passions</strong>, how I have always wanted a <strong>job around my interests</strong>, wanting to <strong>avoid mid life crisis and being unhappy</strong> like so many highly paid but overworked professionals are and <strong>being a self-taught, DIY person</strong>.

Published a blog, wanted to customize design so I learnt HTML/CSS and some Photoshop design, moved to my own site, wanted a better server so I learnt to configure a VPS, switched from Apache to Nginx because of traffic etc. I have been breaking and then figuring out how to fix things since I first used my grandfather's new desktop with Windows 98 back in 1999 (broke that and got an earful :P ). Started installing Linux distros (starting from Ubuntu 6.10 when I was 14 years old) and corrupted the Windows partition on my desktop, then had to learn to install Windows and Linux dual boot. Rooted and loaded custom ROMs on each of my Android phones within days of purchase, etc.

At the end of each round, they ask <strong>if you have any questions</strong>. This is as much about checking whether you're eager to work for the company as it is to clear your doubts. I asked about the job profile and performance parameters/career growth in round 1, tools used+role of open source+how open the company is to its employees contributing back to open source in round 2, and why the senior executive is still excited to work in round 3 respectively. Like everything else about this interview, these questions were not planned at all - but doubts I had. Looking back, they align nicely with the theme of each round.

In the last round, I was asked what I was <strong>really proud of</strong>, and I answered about earning since I was 17 and realizing money isn't that important. Beyond basic needs like a roof over your head, clothes and food, money doesn't really have as much value as we think it does. I also saved up most of my earnings for a difficult times in the future rather than spend it mindlessly on useless things.

I read [this Google SRE AMA on Reddit](http://www.reddit.com/r/IAmA/comments/1w1y5m/we_are_the_google_site_reliability_engineering/) just now and I realized that many of the best companies (both large ones and smaller startups) look for attitude over aptitude.

Skills can be taught, technical subjects can be read up but the <strong>curiosity to learn, take things apart, break and fix them cannot be taught</strong>. This is useful in any job but particularly relevant for this one. Read through all the Google employee comments on the above Reddit AMA link, there are so many gems in comments that help in the interview process for any job.