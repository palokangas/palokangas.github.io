---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: home
---

------------------------------------------------------------------------


# Hei

<p class="subtitle">Olen Teemu Palokangas. Olen viestintäalan ammattilainen <a href="cv">(kts. cv)</a>
	ja nykyisin opetan Oulun ammattikorkeakoulun viestinnän tutkinto-ohjelmassa. Eniten tiedän
	journalismista, retoriikasta, viestintäpolitiikasta ja asiaviihteestä.
</p>

<p class="subtitle">Lisäksi olen kohtuullisen nörtti <a href="portfolio">(kts. portfolio)</a> ja kehitän
	itseäni koodariksi ja datatieteilijäksi. ATK-asioissa eniten kiehtovat Full Stack -hommat,
	tiedonlouhinta ja tiedon visualisointi.
</p>

<p class="subtitle">Harrastan pitkien matkojen juoksemista.</p>





## Testausosio. Blogi tekeillä:

<h1>{{ page.title }}</h1>
<ul class="posts">

  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> » <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>