---
layout: post
title:  "Evaluating and changing email provider"
date:   2021-06-13 21:00:00 +0200
categories: email
tags: []
---
**TL;DR: Changed from Gmail to Fastmail, because it has  custom domains, good enough privacy, multiple users, clear and reasonable pricing, good documentation, polished apps, nice webmail interface, good reviews. So far happy about this choice.**

**Longer version:**

I wanted to find an alternative to Gmail. Google's email service is very responsive and search is very good. Not so crazy about the UI/UX. I have had some difficulties understanding how email threads are organized in webmail and sometimes it has been hard to figure out who am I responding to. Also, I think a paid service with privacy focus will serve my purposes better.

Also, I am using different email addresses on different domains and there is some interest in providing email for the family. So I wanted to investigate the options.

So I did some reseach. There are many options, and I would roughly categorize the options this way from user/maintainer perspective, from complex to simple (some of these overlap quite a bit):

**1. Running your own email server**

Nope. Life is just too short for that.

**2. Email services provided by web hosting package**

I initially thought this was the way to go. And it is a good option. It is very cheap, you have quite a lot of control over the service and lot of space for messages. Also, Roundcube, which is the de facto webmail interface on most platforms is a pretty nice client. You also get very simple instructions on how to configure email to different mobile and desktop clients.

For me, the dealbreaker was the fact that my web hosting deal includes email addresses for two domains. And the next level (4 domains) would have doubled the price, including "too much" web hosting cost just to get the two extra domains. So I wanted to figure out something else.

**3. Using dedicated email services with multi-user and multi-domain support**

This is an interesting and very configurable option, and many services are also very affordable. It is not very easy to figure out what you actually get (Do you get one or multiple custom domains? Do you get multiple users as well? How the pricing really works?). Some services I researched include [Migadu](https://www.migadu.com/), [Mailbox.org](https://mailbox.org), [Runbox](https://runbox.com/), [MXRoute](https://mxroute.com/), [Mailcheap](https://www.mailcheap.co/), [Zoho Mail](https://www.zoho.com/mail/) and [Fastmail](https://fastmail.com).

Out of these Mailbox.org, Zoho Mail and Fastmail also have a "simple" personal email service but they all can be configured for multiple domains and multiple users. The rest of the pack seem more technical and have the advantage of being cheaper (Cheapest Mailcheap shared server started from €2 per month, MXRoute had a special lifetime 10GB package for $125).

Most options include calendar and contacts. Some include more. On the extreme Zoho can be a full office environment if you so choose (also pretty complex to figure out what you actually get if you just want email with domains and users...). I think it makes sense to get contacts/address book functionality, and calendar is nice. I don't really want presentation software or CRM with my email.

Fastmail rose to the top for having good reviews on many forums, good documentation, very polished webmail interface and nice and fully functional iOS apps. Also, the pricing scheme and options were clear. They are not super-privacy oriented but they do focus on providing a service for email users, not advertisers.

**4. Privacy-focused email services**

These services put more focus on privacy. I checked out [ProtonMail](https://protonmail.com/), [Posteo](https://posteo.de/en) and [Tutanota](https://tutanota.com).

I am not involved in stuff that needs super private communications. If I were still in journalism and/or handling very sensitive issues in my work, all of these would be good choices. But for me, it is enough to keep my email out of the hands of advertisers. And also, if I were involved in something very sensitive, I might want to introduce still another level of encryption on top of what these services provide.

For Tutanota and ProtonMail, you need to use their own email clients to ensure end-to-end security. ProtonMail also has "bridge" component that can translate their email to IMAP but they guarantee support only for the most obvious email clients.

For Posteo, you can use IMAP but you cannot get custom domains.

So even though I like the idea of strict privacy, I would rather use my own domain name and use my favourite email clients ([Mutt](http://www.mutt.org/), [MailMate](https://freron.com/)) if I so choose.

**5. Personal email services**

Gmail, Hotmail, Yahoo, iCloud email and the like are all here. I would just rather buy my email service from a company who primarily does email as a product and not because of advertising or by hooking me into some ecosystem. So I did not really think of these as options.

[Hey](https://www.hey.com/) is of course the new kid in town and has nicely brought new dynamics to fairly boring world of email. Hey is also a bit on the expensive side. But ana real dealbreaker for me is that Hey cannot import my existing email. I don't really understand why. If they can receive "standard" email from other users, why can't they receive my existing standard email from me? I have a very big archive of old email that I frequently search and use. I don't want to start over. I am also a bit hesitant on locking in "non-standard" email. So no Hey for me.

As mentioned before, many of the services in previous categories also have a simple options that you can use as "personal email". Fastmail is a good example. You don't really need to use the extra features it provides and don't need to care. But the options are there if you want.

**Conclusion**

So Fastmail it is. Some experiences:

Creating an email account is super easy. They also have 30 day evaluation period and you have access to fully functional product without any commitment (no credit card needed). So very easy to get started and if you only want email address with one of their supported domain names, you don't need more. However, if I only wanted an alternative personal email, there are cheaper and free options.

Configuring DNS records to use your own domains needs some technical skills. The instructions are good and there are even customized instructions for some of the biggest domain registrars (didn't help in my case since I don't use one of those), but you still need to have an understanding of what you are doing. Like what to do with the existing MX, CNAME, TXT and A records, what TTL means and how to troubleshoot issues etc. Fastmail documentation is tutorial-like and to actually understand things and solve problems you will need other resources as well.

The mobile apps are fully featured and include email, calendar, contact, notes and files. These actually work really nicely together in the app and you can do advanced stuff with them. Using contacts and contact groups together with labels and filtering gets you some great workflows (like this Hey-influenced one that I modified for my use, by [Piet van Zoen](https://piet.me/my-email-workflow-in-fastmail/)). This level of tight integration with contacts, groups, labels and rules was a nice surprise that I was not expecting but am delighted to use. This might actually justify the price of the service even for those who only want personal email.

Importing existing email is supported directly by the service and this worked flawlessly in my case. Email, contacts and calendars were all imported from Google services without problems. I have previously migrated email using a desktop client and that can be pretty slow and error prone so this functionality was very much appreciated. I guess Gmail is a prime competitor so they want that migration be as smooth as possible. I did not check if other than Google services are imported as smoothly.

Even though I now have to pay for email service (about €4.30/month with my plan), so far I am very happy about this change. But what also delights me is that I am not locked in in anyway. I can also easily export stuff, and if Fastmail suddenly decides to become evil and nasty (it does not seem that they would), I can just export my stuff and redirect my MX records somewhere else.









