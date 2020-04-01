---
layout: post
title:  "Queen's Greatest Hits 4 and Spotify API"
date:   2020-03-28 18:36:00 +0200
categories: programming
tags: [api, bot, python]
---

Corona epidemic makes people spend time on silly stuff. So a colleague introduced me to a challenge of making a list of Queen's Greatest Hits that are NOT already on existing Greatest Hits 1, 2 or 3.

I grew up with Queen and it is still **The Band** in my records. So I immediately knew that this challenge is not going to be an easy one to pass. But instead of qualitative, personal solution that other people came up with, I decided to go for an automated, democratic solution.

So what I did (reference to relevant information below the list), was to create a small bot that automatically checks all the Queen song versions in Spotify (n = 1637), ranks them by popularity, removes the ones that are already on other Greatest Hits albums and maintains an updating playlist of these hits. For everybody's enjoyment, here it is:

<figure class="image is-1by1">
  <iframe class="has-ratio" width="600" height="800" src="https://open.spotify.com/embed/playlist/0nYk598AxbNWBYNDQO7Y73" frameborder="0" allowfullscreen></iframe>
</figure>

<br />

#### For reference, steps and tools:

- Need to create a client_id and an app in [developer.spotify.com](https://developer.spotify.com/dashboard/applications).
- Access the [Spotify Web API](https://developer.spotify.com/documentation/web-api/) RESTfully. There are many ways. I decided to use [Spotipy](https://spotipy.readthedocs.io/en/2.9.0/) Python library, which seemed quite robust and is well documented, with ample examples.
- Get all Queen albums and tracks through the API.
- Identify unique song names. For example all of these should have same unique song identity: "Doing Alright", "Doing Alright - Remastered 2011", "Doing All Right - Live at the BBC" etc... (VERY TEDIOUS and TIME CONSUMING half-automated/half-manual work).
- Get popularity value for songs. Important note: Spotify API does not currently expose play count for songs. Instead, it uses a its own [popularity](https://developer.spotify.com/documentation/web-api/reference/tracks/get-track/) rating (1-100) based *"in the most part, on the total number of plays the track has had and how recent those plays are"*. The problem here is that *"duplicate tracks (e.g. the same track from a single and an album) are rated independently."* For a band like Queen, whose songs have a very large amount of versions (original, remastered, live, soundtrack, another live, single, demo, mix etc.) this is a problem. The best way would be to sum up the play count of all versions of a song and use that. Instead, I ended up using the most popular version of each song (this might explain why a "song" like "Ay-Oh" gets to the list. There is only one version and it is on a recent, popular soundtrack album). I hope Spotify will open the play count numbers through the API in the future. Else, possibly breaking [workarounds](https://github.com/evilarceus/Spotify-PlayCount) should be made.
- Create a sorted list of all songs based on their popularity, keep only the most popular version of a song and discard the rest, weed out the songs that are already on Greatest Hits 1, 2 and 3. Select 20 songs that are still on the top of this list.
- Authenticate a user (me) and access that user's profile. Create (once) a playlist and update this playlist (daily cronjob) to maintain an up-to-date version of the list when Spotify users' listening habits change.
-  Done

#### Evaluation of results

- *Love of my life* is by far the most popular song that is not on the Greatest Hits albums. Why is it not on those albums? Eludes me.
- *Ay-Oh* (Freddie Mercury engaging the crowd of Live Aid) is track number 18 on Bohemian Rhapsody movie soundtrack album and secondmost popular song on this list. It being so popular has to mean that people actually go and select that song from the list and play it. Or else, Spotify popularity algorithm is playing some tricks here.
-  *Cool Cat* is a disco song on a not-so-good Queen album. It being so high on this list is a big surprise.
-  *My Fairy King* BBC Sessions recording is a delightful surprise.
-  Nice to have older hardish rock era Queen songs on the list (*Brighton Rock*, *Sheer Heart Attack*, *Stone Cold Crazy*...)
- Queen easily has enough good songs to fill a Greatest Hits 4.



